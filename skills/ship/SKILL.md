---
name: ship
description: |
  Ship workflow: merge base branch, run tests, run /review, bump version, update
  changelog, commit, push, create PR. Use when asked to "ship", "create a PR",
  "push to main", "deploy this", "get it merged". Proactively suggest when the user
  says code is ready or wants to push changes up.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - Agent
  - AskUserQuestion
  - WebSearch
triggers:
  - ship it
  - create a pr
  - push to main
  - deploy this
  - get it merged
---

## Ship Workflow

### Step 1 — Assess current state

```bash
git status
git branch --show-current
git log --oneline -5
git diff --stat HEAD
```

If there are uncommitted changes, ask: "Should I commit these first, or are you shipping what's already committed?"

### Step 2 — Merge base branch

```bash
BASE=$(git remote show origin 2>/dev/null | grep "HEAD branch" | awk '{print $NF}' || echo "main")
git fetch origin
git merge origin/$BASE --no-edit 2>/dev/null || echo "Conflicts need resolution"
```

If merge conflicts, stop and ask the user to resolve them.

### Step 3 — Run tests

```bash
# Detect and run the right test command
if [ -f package.json ]; then
  cat package.json | grep -E '"test"|"check"' | head -5
fi
# Run tests — ask user for the right command if unclear
```

If tests fail, stop. Do not ship broken code.

### Step 4 — Run /review

Invoke the `/review` skill. Do not proceed if there are CRITICAL or HIGH issues unless the user explicitly says to continue.

### Step 5 — Version bump (if applicable)

Check if the project has a VERSION file or version in package.json:

```bash
cat VERSION 2>/dev/null || cat package.json | grep '"version"' | head -1
```

Ask: "Should I bump the version? Current: X.Y.Z → What should it be?"

### Step 6 — Update CHANGELOG

If a CHANGELOG.md exists, prepend an entry:

```
## [X.Y.Z] - YYYY-MM-DD

### Added
- ...

### Fixed
- ...
```

### Step 7 — Final commit

```bash
git add -A
git commit -m "$(cat <<'EOF'
<type>: <description>

<body if needed>
EOF
)"
```

### Step 8 — Push and create PR

```bash
git push -u origin $(git branch --show-current)
```

Create PR with:
- Title: concise, imperative mood
- Body: what changed, why, how to test
- Link to any related issues

```bash
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
- ...

## Test plan
- [ ] ...
EOF
)"
```

Output the PR URL.
