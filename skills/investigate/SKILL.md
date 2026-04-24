---
name: investigate
description: |
  Systematic debugging with root cause analysis. Four phases: investigate, analyze,
  hypothesize, implement. Iron Law: no fix without confirmed root cause. Use when
  asked to "debug this", "fix this bug", "why is this broken", "investigate this error".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
  - WebSearch
triggers:
  - debug this
  - fix this bug
  - why is this broken
  - root cause analysis
  - investigate this error
---

## Iron Law

**Never implement a fix until the root cause is confirmed.** Treating symptoms without understanding the cause wastes time and introduces new bugs.

## Phase 1 — Investigate

Reproduce first. Collect raw data before theorizing.

```bash
# Read exact error messages
# Check recent git changes that might have introduced the issue
git log --oneline -20
git diff HEAD~5 HEAD --stat

# Check application logs
# Identify entry point and trace execution path
```

**Questions to answer:**
- What exactly is the error? (exact message, stack trace, line number)
- When did it start? (after which commit/deployment/change)
- Is it reproducible? Under what conditions?
- What environment? (production, staging, local — and do they differ?)

## Phase 2 — Analyze

Read the code from entry point to failure point. Do not skip.

- Read the full file containing the error
- Read all files in the call chain
- Check the git blame on failing lines
- Look for similar patterns in the codebase that work correctly

## Phase 3 — Hypothesize

Form ranked hypotheses. State each as: "The bug is caused by X because Y."

Test each hypothesis with the smallest possible experiment before accepting it.

**Do not accept the first plausible explanation.** Ask "why?" five times.

```
Symptom → Cause 1 → Cause 2 → Cause 3 → Cause 4 → Root cause
```

## Phase 4 — Implement

Only after root cause is confirmed:

1. Fix the root cause, not the symptom
2. Write a test that fails before the fix and passes after
3. Verify the fix in the same environment where the bug appeared
4. Check for similar bugs elsewhere in the codebase

## Output

```
## Investigation: <issue description>

**Root Cause:** <one sentence>

**Evidence:**
- <what you observed>
- <what confirmed the hypothesis>

**Fix:** <what was changed and why>

**Tests added:** <test description>

**Similar issues to check:** <list if any>
```
