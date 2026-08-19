# Cosmic Drop — Project Guide for Claude Code

A single self-contained HTML game (Matter.js physics + Firebase leaderboard). No build step.

## ⛔ CRITICAL RULE — DO NOT READ THE AUDIO DATA (v11 and earlier only)
This only applies to `archive/Cosmic_Drop_vN.html` files where **N ≤ 10**. Those files
end with two huge base64 WAV blobs (`DROP_B64`, `MERGE_B64`) under the
`// ── AUDIO DATA ──` marker comment. These consume enormous token budgets and
**never need to be read or edited.**

- **Never** read past the `AUDIO DATA  (huge base64 — do NOT read or edit)` marker.
- When reading the file, cap your range at that marker line (use `grep -n "AUDIO DATA"`
  to find it, then read only up to it).
- When grepping, the blobs are single massive lines — avoid commands that print them.

All editable logic lives **above** that marker.

From v11 onward (including `index.html`, `test/index.html`, and every `archive/Cosmic_Drop_vN.html`
with N ≥ 11), sounds load via `fetch()` from `sounds/*.mp3` instead — there's no base64 blob,
so this rule doesn't apply and the whole file is safe to read normally.

## File, Versioning & Hosting
- `index.html` — the canonical game file (branded "Galaxy Edition"). Always a copy of the latest archive version.
- **Versioning workflow (always follow):**
  1. Write the new version as `archive/Cosmic_Drop_vN.html` (next number).
  2. Bump the hardcoded version label in the settings menu (grep `pointer-events:none;">v`) to match.
  3. Copy it to `index.html` — **never edit `index.html` directly.**
  Why: clean version history in `archive/`; `index.html` stays a copy of the latest.
- **Current version = the highest-numbered `archive/Cosmic_Drop_vN.html`** (confirm with `git log --oneline`). Never hard-code the current version number in docs — derive it, so it can't go stale across terminals.
- **Repo:** https://github.com/BrandonsCodeIsGreat/cosmic-drop — deploys automatically from the `master` branch on push.
- **Live URLs:** https://cosmic-drop.onrender.com (Render, primary) and https://brandonscodeisgreat.github.io/cosmic-drop (GitHub Pages, via `.github/workflows/deploy-pages.yml`).
- **Test zone:** `test/index.html` — experimental build, served at https://brandonscodeisgreat.github.io/cosmic-drop/test/ by the same Pages workflow (it uploads the whole repo, so no extra infra). Isolation: localStorage keys carry a `_test` suffix, the Firebase leaderboard points at `scores_test/` (currently denied by DB rules → leaderboard is local-only in test), sounds load from `../sounds/`. The test build is OUTSIDE the archive versioning flow — promote a finished feature by folding its diff into the next `archive/Cosmic_Drop_vN.html`.
- **Pending decisions / open questions live in `NOTES.md`** at the repo root (e.g. the leaderboard `_v4`→`_v5` reset). Check it before competitive-scoring changes.
- **Sounds:** `sounds/drop.mp3` + `sounds/merge.mp3`, loaded via `fetch()` (since v11; replaced inline base64 blobs). `archive/device-measure.html` measures a device's real playfield size.

## Architecture
Single HTML file. CDN deps only: Matter.js (physics), Firebase compat SDK v10.12.0 (leaderboard).
The script opens with a **FILE MAP** comment block listing all sections in order —
search `── NAME` to jump to a section.

## Game Modes (`gameSettings.mode`)
| Mode | Cooldowns | Powerup uses | Score submitted |
|---|---|---|---|
| `competition` | none | 5 total/game | yes |
| `zen` | normal | unlimited | no |
| `nocooldown` | none | unlimited | no (easter egg) |

`nocooldown` easter egg: toggle Competition↔Zen 10× within 30s in Settings.
Other: `sizeScale` 0.65/0.85/1.0 (small/med/large), `reverseMode` bool.
`gameSettings` is read at `setupGame()` time — changing mid-game has no effect.

## Planets (tiers 0–10)
Asteroid, Moon, Mercury, Mars, Venus, Earth, Neptune, Uranus, Saturn, Jupiter, Sun.
`BASE_RADII = [14,20,27,35,44,54,65,77,90,104,120]`. Tier 0 never scales;
others = `round(BASE_RADII[tier] * sizeScale)`. Drop tiers: 0–4 normal, 6–10 reverse.

## Scoring
Merge tier-X → `(X+1)×10`. Create Sun +500. Supernova (two Suns) +2000.
EVOLVE non-Sun `(tier+2)×5`, on Sun +500.
Reverse: Singularity (two Asteroids) +1000. SHRINK tier>0 `(11−tier)×5`, on Asteroid +200.
ZAP/Sweep/Comet/Magnet/Quake give no direct score; merges they cause score normally.

## Reverse Mode
Drop tiers 6–10, merge **down** one tier. Two Asteroids → SINGULARITY (+1000).
EVOLVE button becomes SHRINK. Sweep removes biggest tiers (≥8) not smallest.
Evolution guide reverses. Mass-proportional upward push for low result tiers.

## Competition Charges
`COMPETITION_USES = 5`, shared across powerups. Tracked in `powerupUsesLeft`,
reset in `setupGame()`. Charge consumed at moment of effect, not button press.
`useCompetitionCharge()` returns false at 0. Gold-dot pill overlay (zero layout space).

## Danger / Game Over
`DANGER_Y = 95`. Dangerous if `position.y >= 0` AND top above threshold AND `|velocity.y| < 0.5`.
The `y >= 0` guard stops airborne balls (merge push / comet) triggering game over.

## Leaderboard / Firebase
Structure: `scores/{size}/{pushId}`, size from `getSizeKey()` (≤0.65 small, ≥1.0 large, else medium).
LocalStorage: `cosmicDropScores_{small|med|large}_v4`. `FIREBASE_CONFIG` constant near top is live.
Submission only in `competition` mode. 3 leaderboard tabs default to size just played.

## Powerups
SWEEP (`sweep-btn`), ZAP (`destroy-btn`), EVOLVE/SHRINK (`upgrade-btn`),
COMET (`comet-btn`), MAGNET (`magnet-btn`), QUAKE (`quake-btn`).
CD functions return 0 for competition/nocooldown, `*_CD_BASE` for zen.

## Conventions
- `setupGame()` resets all state (score, world, CDs, charges, effects, guide, badges, button labels).
- `returnToMenu()` resets evolve button + hides reverse badge.
- `buildEvolutionGuide()` reads `reverseMode`. `drawSizePreview()` renders size preview.
- Easter egg state persists for the browser session.
