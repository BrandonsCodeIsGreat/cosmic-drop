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

## Other open question

- Baseline height is **570**, which includes the ~53px strip that sits under the
  mobile browser toolbar (measured `visualViewport` was 517). Consistent for
  everyone, but if we'd rather the baseline be the *visible* height, it's a
  one-number change to `LOGICAL_H`.

## Tooling

- `archive/device-measure.html` — standalone tool to measure a device's real
  playfield size. See the comment at the top of that file. Use it if we ever
  want to re-baseline the field to a different device.
