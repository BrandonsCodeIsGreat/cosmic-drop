---
name: project-cosmic-drop-current-state
description: Current state of Cosmic Drop — live at v13 on GitHub Pages; migrated from Netlify 2026-06-03
metadata:
  node_type: memory
  type: project
  originSessionId: cb494920-6c8f-43a4-ad72-1a828ae81410
---

Cosmic Drop is **live at https://brandonscodeisgreat.github.io/cosmic-drop** at v13.

**Hosting:** Migrated from Netlify (ran out of credits 2026-06-03) to GitHub Pages. Repo: `https://github.com/BrandonsCodeIsGreat/cosmic-drop`. GitHub Pages is enabled on the `master` branch root.

**Shipped versions (archive):**
- v11 — audio refactor: switched from base64 WAV blobs to `fetch()`-based external MP3s; file shrunk from 275KB → ~113KB
- v12 — Neptune favicon, Netlify cache headers (`netlify.toml`), title screen spacing fix
- v12b — minor polish on top of v12
- v13 — migrated to GitHub Pages; fixed favicon path (removed leading `/`); updated share card footer URL and share text to include new URL with game link

**Share card:** Footer shows `brandonscodeisgreat.github.io/cosmic-drop`. `navigator.share` text includes the URL so it autopopulates in Messages when sharing on mobile.

**Sounds:** `sounds/drop.mp3` (2KB) and `sounds/merge.mp3` (5KB) — external files, loaded via fetch.

**Why:** Base64 WAV blobs were ~160KB each and consumed ~40K tokens when read. Now gone.
