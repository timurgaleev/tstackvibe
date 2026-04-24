---
name: freeze
description: |
  Freeze scope — prevents new features or refactors from being added to the current
  task. Only bug fixes and the originally defined work are allowed. Use when you
  want to prevent scope creep: "freeze scope", "no new features", "stick to the plan".
allowed-tools:
  - Bash
  - Read
  - AskUserQuestion
triggers:
  - freeze scope
  - no new features
  - stick to the plan
  - scope freeze
---

## Scope Freeze Active

**The scope is now frozen.** Only the following are allowed:
- Fixes to bugs introduced by the current work
- Tests for the current work
- Documentation for the current work

### Blocked until /unfreeze

- New features
- Refactoring code outside the current task
- "While I'm here" changes
- Performance improvements not required by the current task
- Dependency updates not required by the current task

### When something outside scope is identified

Instead of making the change, log it:

```bash
echo "DEFERRED: <description> — found during <current task>" >> docs/backlog.md
```

Then continue with the frozen scope.

### Checklist for any proposed change

```
□ Is this change required to complete the current task? 
  If yes: proceed
  If no: defer it, log it, stay frozen
```

### Deactivate

Run `/unfreeze` to lift the freeze.
