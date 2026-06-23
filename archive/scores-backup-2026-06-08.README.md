# scores-backup-2026-06-08.json

Full snapshot of the Firebase Realtime Database `/scores` node, taken **2026-06-08**
immediately **before** a leaderboard wipe. This is the complete pre-wipe board across
all three size tiers (`small` / `medium` / `large`), 34 entries total.

## Why it exists
On 2026-06-08 the shared leaderboard was trimmed to keep only scores recorded
**after 2:00 PM EDT** (the post-fixed-playfield window). Scores were classified by
decoding the timestamp embedded in each Firebase push ID (first 8 chars), cross-checked
against the human-readable `date` field. This file preserves the entries that were
removed (and everything else) so they can be restored if needed.

## Structure
Same shape as the live DB node:

```json
{ "<size>": { "<pushId>": { "name": "...", "score": 1234, "date": "Jun 8, 2026" } } }
```

## Restoring
Write the JSON back to the `/scores` path of the
`https://cosmic-drop-default-rtdb.firebaseio.com` database (Firebase MCP
`realtimedatabase_set_data`, or the Firebase console import). Note: a full `set` on
`/scores` **replaces** the entire node, so merge manually if the live board has newer
scores you want to keep.
