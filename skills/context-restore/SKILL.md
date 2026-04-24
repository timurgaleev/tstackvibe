---
name: context-restore
description: |
  Restore working context saved by /context-save. Loads the most recent saved
  state so you can pick up exactly where you left off — even across sessions.
  Use when asked to "resume", "restore context", "where was I",
  "pick up where I left off".
allowed-tools:
  - Bash
  - Read
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - resume where i left off
  - restore context
  - where was i
  - pick up where i left off
  - context restore
---

## Context Restore Workflow

### Step 1 — Find saved contexts

```bash
# Look in current project
find .tstackvibe/ -name "context-*.md" 2>/dev/null | sort -r | head -10

# Also check if there's one for the current branch
BRANCH=$(git branch --show-current 2>/dev/null || echo "")
find .tstackvibe/ -name "context-${BRANCH}-*.md" 2>/dev/null | sort -r | head -5
```

### Step 2 — Select context

If multiple found, show the list and ask:
> "Found these saved contexts. Which one? (default: most recent)"

If none found:
> "No saved context found in `.tstackvibe/`. Is the context file somewhere else, or should we start fresh?"

### Step 3 — Load and present

Read the selected context file completely.

Present a structured summary:

```
## Resuming: <branch> — <saved date>

**Task:** <what we were building>

**Progress:**
- Done: <list>
- In progress: <list>
- Remaining: <list>

**Key decisions:**
- <decisions>

**Blockers (if any):**
- <blockers>

**First action to take now:**
<suggested next step based on "remaining" and "in progress">
```

### Step 4 — Verify git state

```bash
git branch --show-current
git status --short
git log --oneline -5
```

If the current branch doesn't match the saved context: "Current branch is `<x>` but context was saved on `<y>`. Switch branches first?"

### Step 5 — Resume

Ask: "Ready to continue? I'll start with: <first remaining task>."
