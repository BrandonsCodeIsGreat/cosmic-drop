---
name: cosmic-drop
description: Cosmic Drop project — versioning workflow, hosting, and current state
metadata:
  type: project
---

**Live at:** https://cosmic-drop.onrender.com (deployed on Render 2026-06-03). Also accessible via GitHub Pages at https://brandonscodeisgreat.github.io/cosmic-drop.
**Repo:** https://github.com/BrandonsCodeIsGreat/cosmic-drop — deploys automatically from `master` branch on push.

**Hosting history:** Started on Netlify → moved to GitHub Pages (2026-06-03, ran out of Netlify credits) → added Render.com deployment (2026-06-03).

**Versioning workflow:** Always follow this pattern when editing game files:
1. Write the new version as `archive/Cosmic_Drop_vN.html` (next version number)
2. Copy it to `index.html` — never edit `index.html` directly
**Why:** Clean version history in archive; index.html is always a copy of the latest archive version.

**Shipped versions (archive):**
- v11 — audio refactor: switched from base64 WAV blobs to `fetch()`-based external MP3s; file shrunk from 275KB → ~113KB
- v12 — Neptune favicon, `netlify.toml` cache headers, title screen spacing fix
- v12b — minor polish on top of v12
- v13 — migrated to GitHub Pages; fixed favicon path; updated share card footer URL and share text

**Sounds:** `sounds/drop.mp3` (2KB) and `sounds/merge.mp3` (5KB) — external files, loaded via fetch.

**Share card:** Footer shows the game URL. `navigator.share` text includes URL so it autopopulates in Messages on mobile.
