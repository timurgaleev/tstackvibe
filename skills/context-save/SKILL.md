---
name: context-save
description: |
  Save working context. Captures git state, open tasks, decisions made, and
  remaining work so any future session can pick up without losing a beat.
  Use when asked to "save progress", "save state", "context save", "save my work".
  Pair with /context-restore to resume later.
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - save progress
  - save state
  - save my work
  - context save
  - checkpoint
---

## Context Save Workflow

### Step 1 — Collect state

```bash
# Git state
git branch --show-current
git status --short
git log --oneline -10
git stash list 2>/dev/null | head -5

# What's in progress
git diff --stat HEAD
```

### Step 2 — Ask for context

> "What are you working on? What decisions have been made? What's left to do?"

Prompt for any non-obvious context that git history won't capture:
- Why a certain approach was chosen over alternatives
- Blocked items and what's blocking them
- Things discovered mid-task that changed direction

### Step 3 — Write context file

Save to `.tstackvibe/context-<branch>-<timestamp>.md`:

```bash
mkdir -p .tstackvibe
BRANCH=$(git branch --show-current 2>/dev/null || echo "no-branch")
TIMESTAMP=$(date +%Y%m%d-%H%M)
CONTEXT_FILE=".tstackvibe/context-${BRANCH}-${TIMESTAMP}.md"
```

Content:

```markdown
# Context — <branch> — <date>

## Git state
- Branch: <branch>
- Last commit: <hash> <message>
- Uncommitted changes: <files>

## What we're building
<description of the current task>

## Decisions made
- <decision> — because <reason>
- ...

## Progress
### Done
- ...

### In progress
- ...

### Remaining
- ...

## Blockers
- ...

## Context for next session
<anything the AI needs to know that isn't in the code>

## Resume command
```bash
git checkout <branch>
# Then: /context-restore
```
```

### Step 4 — Confirm

Show the saved file path and say: "Context saved. Run `/context-restore` in any session to pick up here."

Also offer: "Want me to commit this context file?"
