---
name: qa-only
description: |
  QA report only — finds bugs but does not fix them. Produces a prioritized bug list.
  Use when you want an audit without code changes, or when bugs need separate PRs.
  For test-and-fix mode, use /qa instead.
allowed-tools:
  - Bash
  - Read
  - Glob
  - Grep
  - AskUserQuestion
  - WebSearch
triggers:
  - qa report only
  - audit bugs
  - find bugs without fixing
  - qa audit
---

## QA Audit Workflow

Same test plan as `/qa` but output only — no code changes.

### Step 1 — Scope

Ask: "What URL/feature/flow to audit? Scope: Quick / Standard / Exhaustive?"

### Step 2 — Read the code

```bash
git log --oneline -5
git diff HEAD~3 HEAD --stat
```

Read relevant source files to understand intended behavior.

### Step 3 — Execute test plan

Follow the same test plan structure as `/qa`. Execute each test case mentally (reading code + tracing logic) or against a running instance.

### Step 4 — Report only

```
## QA Audit: <feature/URL>

**Tier:** Quick / Standard / Exhaustive
**Date:** YYYY-MM-DD
**Mode:** REPORT ONLY — no changes made

### Critical bugs (block release)
1. **<description>** — File: <file:line> — Steps to reproduce: ...

### High bugs (fix before next release)
1. ...

### Medium bugs (fix in next sprint)
1. ...

### Low / cosmetic
1. ...

### Health score: X/10

### Recommended: SHIP / HOLD / BLOCK
```

After report: "Want me to switch to /qa mode to fix these?"
