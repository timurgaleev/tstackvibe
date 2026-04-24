---
name: land-and-deploy
description: |
  Land and deploy workflow. Merges a PR, monitors CI, waits for deploy to complete,
  runs a /canary health check on production. Takes over after /ship creates the PR.
  Use when: "merge the PR", "land it", "deploy to production", "land and deploy".
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
  - AskUserQuestion
triggers:
  - merge and deploy
  - land the pr
  - ship to production
  - land it
  - deploy to production
---

## Land and Deploy Workflow

### Step 1 — Confirm PR is ready

```bash
# Find the open PR for current branch
BRANCH=$(git branch --show-current)
gh pr view --json number,title,state,mergeable,statusCheckRollup 2>/dev/null
```

Check:
- PR is approved
- All CI checks are passing (green)
- No merge conflicts
- No unresolved review comments

If any check fails: "PR is not ready to merge. Issues: <list>"

### Step 2 — Confirm with user

> "About to merge PR #<N>: '<title>' into <base>. CI: <status>. Proceed?"

**STOP — wait for explicit confirmation before merging.**

### Step 3 — Merge

```bash
gh pr merge <PR_NUMBER> --merge --delete-branch 2>/dev/null || \
gh pr merge <PR_NUMBER> --squash --delete-branch 2>/dev/null
```

Prefer `--merge` (preserves history). Fall back to `--squash` if repo policy requires it.

### Step 4 — Monitor CI / deploy

```bash
# Watch the merge commit's CI status
git fetch origin
git log origin/main --oneline -3

# If there's a deploy pipeline, check its status
gh run list --limit 5 2>/dev/null
gh run watch 2>/dev/null || echo "Monitor your deploy pipeline manually"
```

Poll until deploy completes (or ask user to confirm when it's done).

### Step 5 — Production canary check

Once deployed, run `/canary` to verify:
- Error rates are at baseline
- Latency is within normal range
- No new errors in logs

```
## Deploy check: <service> <version>

CI: PASSED
Deploy: COMPLETED
Canary: HEALTHY / DEGRADED / ROLLBACK

Production health: GREEN / YELLOW / RED
```

### Step 6 — Wrap up

If healthy:
> "Deploy complete. Production is healthy. PR #<N> is live."

If degraded:
> "Deploy complete but canary shows issues. Recommend running /canary for a full check. Consider rollback: `git revert HEAD && git push`"
