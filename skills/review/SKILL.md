---
name: review
description: |
  Pre-landing PR code review. Analyzes the diff against the base branch for bugs,
  SQL safety, security issues, logic errors, and structural problems. Use when asked
  to "review this PR", "code review", "check my diff", or before merging.
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Agent
  - AskUserQuestion
  - WebSearch
triggers:
  - review this pr
  - code review
  - check my diff
  - pre-landing review
---

## Review Workflow

### 1. Gather context

```bash
git branch --show-current
git log --oneline -10
git diff $(git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null || echo "HEAD~1") HEAD --stat
```

### 2. Read the full diff

```bash
git diff $(git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null || echo "HEAD~1") HEAD
```

Read every changed file end-to-end before forming opinions. For context, read the unchanged surrounding code in files with large changes.

### 3. Analyze systematically

Check each category in order. Do not skip categories even if changes seem small.

**Correctness**
- Logic errors, off-by-one errors, wrong conditions
- Unhandled edge cases or null/undefined paths
- Race conditions, concurrency issues

**Security**
- SQL injection (parameterized queries only)
- XSS (output encoding)
- Secrets or credentials in code
- Input validation missing
- Authorization checks bypassed

**Database safety**
- Missing transactions on multi-step writes
- Missing indexes on foreign keys or query columns
- Irreversible migrations without a rollback plan
- N+1 query patterns

**LLM trust boundary** (if applicable)
- Prompt injection vectors
- User-controlled content reaching system prompts
- Output parsed without validation

**Error handling**
- Unhandled promise rejections or exceptions
- Silent failures masking bugs
- Errors exposing stack traces to users

**Tests**
- Changed behavior without updated tests
- New code without test coverage
- Tests that always pass regardless of implementation

**Code quality**
- Unnecessary complexity
- Code that will confuse the next reader
- Duplication of existing utilities

### 4. Output format

```
## Review: <branch-name>

### Critical (block merge)
- [FILE:LINE] Description of issue

### High (fix before merge)
- [FILE:LINE] Description of issue

### Medium (fix or note)
- [FILE:LINE] Description of issue

### Low (optional cleanup)
- [FILE:LINE] Description

### Verdict
APPROVE / REQUEST CHANGES / BLOCK

Confidence: X/10
```

If no issues found in a category, omit it. Always include Verdict and Confidence.

### 5. Offer next steps

After the review, briefly offer: "Want me to fix any of these? I can address them one by one."
