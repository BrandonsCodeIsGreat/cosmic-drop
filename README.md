# Cosmic Drop 🪐

A self-contained planet-merging physics game. Single HTML file, no build step.

## Run
Open `cosmic-drop.html` in a browser, or serve locally:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000/cosmic-drop.html
```

## Dependencies (CDN, no install)
- Matter.js 0.19.0 — physics
- Firebase compat SDK 10.12.0 — shared leaderboard

## Editing
See `CLAUDE.md` for full architecture, game modes, scoring, and conventions.

**Note:** the file ends with two large base64 audio blobs under the `AUDIO DATA`
marker. Don't read or edit them — all logic lives above that line.
