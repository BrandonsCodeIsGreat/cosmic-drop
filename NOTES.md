# Notes — possible next steps

## Leaderboard reset for fair-field scores (NOT done yet)

As of v16 (2026-06-08) the playfield is a **fixed logical size** for every device
(`LOGICAL_W=344 × LOGICAL_H=570`, frozen from a Galaxy Z Flip 5). Before this,
the field scaled with screen size, so taller phones could stack more planets —
an uneven competitive field.

**The catch:** every score already on the leaderboard was set on the *old*
variable-size field. New scores are now comparable to each other, but not to the
old ones. If we want a clean, fair competitive slate:

- Bump the score storage keys so the board starts fresh on the fixed field:
  - Local: `SCORES_KEYS` in `index.html` — `small/med/large` `..._v4` → `..._v5`.
  - Shared: the Firebase path the leaderboard reads/writes (same version suffix).
- Old scores stay in storage under the `_v4` keys (not lost), just no longer shown.

**Decision pending:** do this now, or wait until more people have played the
fixed field? Holding off for now per Brandon (2026-06-08).

## COMET rework vs. the existing boards (v23) — DECIDED, no reset

v23 replaced the slingshot COMET with the full-screen line-sweep: one charge now
clears every planet along a player-drawn line, where before it was a single
physical ball. In `competition` mode that charge comes from the same pool of
`COMPETITION_USES = 5`, so the powerup's value per charge changed materially.
That makes v23 scores and v22-and-earlier scores not strictly comparable.

**Decided (2026-08-19, per Brandon): v23 scores share the existing `_v4` boards.**
No key bump for the COMET change — the live board carries straight over. This is
the current behaviour, so nothing in the code needed changing; v23 reads and
writes `scores/{size}/{pushId}` exactly as v22 did.

Note this decision covers the COMET rework only. The fixed-playfield question
above is still open and untouched.

## Other open question

- Baseline height is **570**, which includes the ~53px strip that sits under the
  mobile browser toolbar (measured `visualViewport` was 517). Consistent for
  everyone, but if we'd rather the baseline be the *visible* height, it's a
  one-number change to `LOGICAL_H`.

## Tooling

- `archive/device-measure.html` — standalone tool to measure a device's real
  playfield size. See the comment at the top of that file. Use it if we ever
  want to re-baseline the field to a different device.
