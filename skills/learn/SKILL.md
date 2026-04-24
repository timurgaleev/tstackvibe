---
name: learn
description: |
  Capture and persist project learnings — debugging insights, architecture decisions,
  gotchas, patterns that work, patterns that don't. Prevents the team from solving
  the same problem twice. Use when asked to "save this learning", "document this",
  "capture what we learned", "add to learnings".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
triggers:
  - save this learning
  - document this
  - capture what we learned
  - add to learnings
  - remember this
---

## Capture Workflow

### Step 1 — Extract the learning

Ask (or infer from context):
> "What exactly did we learn? Be specific — a vague 'be careful with X' isn't useful. A useful learning is: 'When X happens, do Y, not Z, because of reason W.'"

Good learning structure:
- **Context**: when does this apply?
- **Finding**: what did we discover?
- **Why**: the underlying cause or reason
- **Action**: what to do (or not do) next time

### Step 2 — Categorize

Categories:
- `debugging` — root causes of specific bug patterns
- `architecture` — design decisions and their tradeoffs
- `gotcha` — non-obvious behavior of libraries/tools/platform
- `pattern` — things that work well
- `anti-pattern` — things that don't work
- `process` — team workflow improvements
- `performance` — optimization findings
- `security` — vulnerability patterns found

### Step 3 — Save

Check for existing learnings file:

```bash
ls docs/learnings/ 2>/dev/null || ls docs/ 2>/dev/null
```

Append to `docs/learnings/<category>.md` (create if missing):

```markdown
## <short title> — <date>

**Context:** <when does this apply>

**Finding:** <what we learned>

**Why:** <root cause or reason>

**Action:** <what to do next time>

**Source:** <PR number, incident, debugging session>
```

### Step 4 — Check for duplicates

```bash
grep -r "<key term from the new learning>" docs/learnings/ 2>/dev/null
```

If a similar learning exists, update it rather than adding a duplicate.

### After saving

Offer: "Want me to also add a test or comment to the codebase that prevents this from happening again?"
