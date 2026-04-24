---
name: careful
description: |
  Careful mode — activates extra caution for risky operations: database migrations,
  auth changes, payment code, infrastructure changes, or anything where a mistake
  is hard to reverse. Use when about to do something risky: "be careful", "careful mode",
  "this is risky", "production change".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - AskUserQuestion
triggers:
  - careful mode
  - be careful
  - this is risky
  - production change
  - handle with care
---

## Careful Mode

**Activated for the current session.** Extra checks apply to all operations.

### What changes in careful mode

**Before any write operation:**
1. State exactly what will change
2. State what the rollback plan is
3. Ask for confirmation before proceeding

**Before any database change:**
- Read the migration fully
- Verify there is a rollback migration
- Estimate impact on existing data
- Ask: "Is this reversible? What's the worst case?"

**Before any auth/security change:**
- Read all affected auth middleware
- Trace all code paths that touch the change
- Verify no existing sessions are invalidated unexpectedly

**Before any infrastructure change:**
- State current state and target state
- Identify all systems affected
- Confirm rollback procedure

### Risk checklist (run before each operation)

```
□ What exactly changes?
□ What stays the same?
□ What's the rollback?
□ Who/what is affected?
□ Is this reversible in < 5 minutes?
□ Is this tested in staging first?
```

### When in doubt

Stop. Ask: "I'm about to do X. The risk is Y. Rollback is Z. Should I proceed?"

### Deactivate

Say "exit careful mode" or "/unfreeze" to return to normal operation.
