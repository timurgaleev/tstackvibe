---
name: qa
description: |
  Systematically QA test a web application or feature and fix bugs found. Iteratively
  tests, finds bugs, fixes them in source code, and re-verifies. Use when asked to
  "qa", "test this", "find bugs", "test and fix", "does this work". For report-only
  mode, use /qa-only.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
  - WebSearch
triggers:
  - qa test this
  - find bugs on site
  - test the site
  - test this feature
  - does this work
---

## QA Workflow

### Step 1 — Define scope

Ask if not clear:
> "What URL/feature/flow to test? Any specific scenarios to focus on?"

Choose tier:
- **Quick**: critical paths and obvious breakage only
- **Standard**: full happy path + key error states (default)
- **Exhaustive**: every edge case, every state, every error path

### Step 2 — Understand the feature

```bash
# Read relevant source files
# Check recent changes
git log --oneline -10
git diff HEAD~5 HEAD --stat
```

Read the code for the feature being tested to understand intended behavior.

### Step 3 — Test plan

Define test cases before testing:

**Critical paths (must work)**
- [ ] Primary user action succeeds
- [ ] Data persists correctly
- [ ] Success state is communicated

**Error states (must handle)**
- [ ] Invalid input shows clear error
- [ ] Network failure handled gracefully
- [ ] Empty state displays correctly
- [ ] Boundary values (0, max, empty string)

**Edge cases**
- [ ] Concurrent operations
- [ ] Very long inputs
- [ ] Special characters
- [ ] Rapid repeated actions

**Regression**
- [ ] Adjacent features still work
- [ ] Data from before the change is handled

### Step 4 — Execute tests

For each test case:
1. State what you're testing
2. State the expected result
3. State the actual result
4. Mark PASS / FAIL / BLOCKER

### Step 5 — Fix bugs found

For each FAIL:
1. Read the relevant source code
2. Identify the root cause (not just the symptom)
3. Fix it
4. Re-run the test to confirm fixed
5. Check for regressions

### Step 6 — Report

```
## QA Report: <feature/URL>

**Tier:** Quick / Standard / Exhaustive
**Date:** YYYY-MM-DD

### Results
| Test case | Expected | Actual | Status |
|-----------|----------|--------|--------|
| ... | ... | ... | PASS/FAIL |

### Bugs found and fixed
1. **<bug description>** — Fixed in <file:line>

### Bugs found, not fixed (needs separate PR)
1. **<bug description>** — Severity: HIGH/MEDIUM/LOW

### Health score: X/10
### Verdict: SHIP-READY / NEEDS FIXES / BLOCKED
```
