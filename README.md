# Cosmic Drop 🪐

A planet-merging physics game. No build step.

**Live:** https://cosmicdrop.netlify.app

## Run locally

Requires a local server — audio uses `fetch()` and won't work over `file://`.

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Structure

```
index.html      — game (HTML + CSS + JS, ~113KB)
sounds/
  drop.mp3      — drop sound
  merge.mp3     — merge sound
favicon.png     — Neptune planet icon (64×64)
netlify.toml    — cache headers for Netlify
```

## Dependencies (CDN, no install)
- Matter.js 0.19.0 — physics
- Firebase compat SDK 10.12.0 — shared leaderboard

## Editing
See `.claude/CLAUDE.md` for full architecture, game modes, scoring, and conventions.

Versioning workflow: write changes to `archive/Cosmic_Drop_vN.html` first, then copy to `index.html`.
