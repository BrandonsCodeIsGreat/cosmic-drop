---
name: project-cosmic-drop-audio-refactor
description: Status of the cosmic-drop audio refactor from base64 blobs to external MP3 files
metadata: 
  node_type: memory
  type: project
  originSessionId: cb494920-6c8f-43a4-ad72-1a828ae81410
---

Audio refactor is **in progress but not yet live**. v11 is ready but was reverted because the Claude Code preview panel wasn't clickable (likely a preview quirk, not a real bug).

**What was done (2026-06-02):**
- Extracted drop and merge sounds from `B:\Downloads\Screen_Recording_20260602_221024_Fruit Merge.mp3`
  - Drop sound: 0.72s–0.79s → `sounds/drop.mp3` (2KB)
  - Merge sound: 1.78s–2.13s → `sounds/merge.mp3` (5KB)
- Created `archive/Cosmic_Drop_v11.html` — same as v10c but `initAudio()` now loads sounds via `fetch()` instead of base64 blobs; audio blobs removed entirely
- File shrinks from 275KB → 116KB
- `index.html` was reverted to v10c pending real browser test

**What needs to happen next:**
- Test v11 in a real browser (needs a local server — `fetch()` won't work over `file://`)
- If sounds work: push to GitHub, deploy to Netlify
- `sounds/` folder is already in the repo at `C:\Users\Brandon\cosmic-drop\sounds\`

**Why:** Base64 WAV blobs were ~160KB each, consumed ~40K tokens when read, and were never editable. External MP3s are smaller, cacheable, and keep the HTML clean.
