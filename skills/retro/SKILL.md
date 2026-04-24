---
name: retro
description: |
  Weekly engineering retrospective. Reviews what shipped, what broke, what slowed
  the team down, and sets action items. Use when asked to "run a retro", "weekly
  retrospective", "what did we ship this week", "team retrospective".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Write
  - AskUserQuestion
  - WebSearch
triggers:
  - run a retro
  - weekly retrospective
  - team retro
  - what did we ship this week
---

## Retro Workflow

### Step 1 — Gather data

```bash
# What shipped this week?
git log --oneline --since="7 days ago" --format="%h %s %an"

# What PRs merged?
gh pr list --state merged --limit 20 2>/dev/null || true

# Any open incidents or issues?
gh issue list --state open --label "bug" --limit 10 2>/dev/null || true
```

### Step 2 — Prompt the team (or self-reflect)

Ask these questions one at a time, or fill them in from git history:

**Shipped**
> "What did we complete and ship this week?"

**Proud of**
> "What are we most proud of from this week? What went well?"

**Slowed down**
> "What caused delays or friction? What took longer than expected?"

**Broke**
> "What broke or regressed? How did we find out?"

**Would do differently**
> "If we ran this week again, what would we do differently?"

**Carry forward**
> "What unfinished work carries into next week?"

### Step 3 — Identify patterns

Look for systemic issues, not one-off events:
- Did the same type of bug appear twice? → Missing test category
- Did communication fail at the same point? → Process gap
- Did we scope correctly? → Planning accuracy

### Step 4 — Action items

Each action item must have:
- **What**: specific action, not vague intent
- **Who**: specific person
- **When**: specific date (not "soon")

### Output

Save to `docs/retro/YYYY-MM-DD.md`:

```markdown
# Retro — Week of YYYY-MM-DD

## Shipped
- ...

## Proud of
- ...

## What slowed us down
- ...

## What broke
- ...

## Would do differently
- ...

## Action items
| What | Who | By when |
|------|-----|---------|
| ... | ... | ... |

## Patterns (systemic observations)
- ...
```
