---
name: landing-report
description: |
  PR and version queue dashboard. Shows open PRs, their CI status, version
  numbers, and which is ready to land. Read-only — no mutations. Use when asked
  to "landing report", "what's in the queue", "show open PRs", "ship queue".
allowed-tools:
  - Bash
  - Read
  - AskUserQuestion
triggers:
  - landing report
  - version queue
  - ship queue
  - show open prs
  - whats in the queue
---

## Landing Report

### Collect all PR data

```bash
# Open PRs
gh pr list --state open --json number,title,author,headRefName,mergeable,isDraft,statusCheckRollup,labels 2>/dev/null

# Recent merges
gh pr list --state merged --limit 5 --json number,title,mergedAt 2>/dev/null

# Current version
cat VERSION 2>/dev/null || cat package.json | grep '"version"' | head -1 2>/dev/null

# CI status on main
gh run list --branch main --limit 5 2>/dev/null
```

### Output

```
## Landing Report — <date> <time>

### Current version: <X.Y.Z>

### Open PRs (queue)

| # | Title | Author | Branch | CI | Mergeable | Ready? |
|---|-------|--------|--------|----|-----------|--------|
| #N | <title> | <author> | <branch> | GREEN/RED/PENDING | YES/NO/CONFLICT | ✓ / ✗ |

### Merge-ready (CI green + approved + no conflicts)
- PR #N: <title> — ready to land

### Blocked
- PR #N: <title> — blocked by: <CI failing / needs review / conflicts>

### Recently merged (last 5)
- PR #N: <title> — merged <X hours/days> ago

### Main branch CI: GREEN / RED / PENDING
```

After report: "Want me to merge any of the ready PRs? Run `/land-and-deploy` to proceed."
