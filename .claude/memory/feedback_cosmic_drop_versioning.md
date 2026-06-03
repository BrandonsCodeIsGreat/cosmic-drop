---
name: feedback-cosmic-drop-versioning
description: How Brandon versions Cosmic Drop HTML files when making edits
metadata: 
  node_type: memory
  type: feedback
  originSessionId: cb494920-6c8f-43a4-ad72-1a828ae81410
---

When editing cosmic-drop game files, always follow this versioning workflow:

1. Create the new edited file as `archive/Cosmic_Drop_vN.html` (next version number)
2. The previous version is already in archive — don't touch it
3. Copy the new archive file to `index.html` (what Netlify loads)

Never edit `index.html` directly. Always version → archive → copy to index.

**Why:** Clean version history in archive; index.html is always a copy of the latest archive version.

**How to apply:** Any time a change is made to the cosmic-drop game, bump the version number, write to archive first, then copy to index.html.
