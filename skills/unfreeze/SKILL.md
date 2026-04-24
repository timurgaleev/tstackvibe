---
name: unfreeze
description: |
  Lift a scope freeze. Returns to normal operation after /freeze or /careful mode.
  Use when ready to expand scope again: "unfreeze", "lift freeze", "scope open".
allowed-tools:
  - AskUserQuestion
triggers:
  - unfreeze
  - lift freeze
  - scope open
  - exit careful mode
---

## Scope Unfrozen

Scope freeze has been lifted. Normal operation resumes.

If a backlog was maintained during the freeze, review it now:

```bash
cat docs/backlog.md 2>/dev/null | grep "DEFERRED"
```

Ask: "Want to review the deferred items that were logged during the freeze?"
