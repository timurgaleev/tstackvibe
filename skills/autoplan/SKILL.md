---
name: autoplan
description: |
  Auto review pipeline — runs CEO, design, and eng reviews sequentially with
  auto-decisions using 6 decision principles. Surfaces taste decisions at a final
  approval gate. One command, fully reviewed plan out. Use when asked to "autoplan",
  "run all reviews", "auto review this", "review everything".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - run all reviews
  - automatic review pipeline
  - auto plan review
  - autoplan
---

## Autoplan Workflow

### Step 1 — Load the plan

Find and read the plan:

```bash
find . -name "*.md" | xargs grep -l "plan\|feature\|implement" 2>/dev/null | head -5
```

If no plan exists, ask: "Paste the plan or describe what you're building."

### Step 2 — CEO Review (auto-mode)

Apply 6 auto-decision principles:

1. **Default to HOLD SCOPE** unless the feature is clearly underscoped
2. **Cut anything with a risk:reward > 2:1** (high risk, unclear reward)
3. **Prefer boring technology** over novel choices when both work
4. **Narrow the wedge** — the MVP should serve one user type extremely well
5. **Reject "nice to have"** — if it's not painful to omit, omit it
6. **Flag taste decisions** — scope, UX direction, positioning → surface to user

Run /plan-ceo-review mentally. Auto-decide everything except taste decisions.

### Step 3 — Design Review (auto-mode)

Run /plan-design-review mentally. Auto-apply:
- Minimum steps to complete primary task
- No jargon in UI text
- Mobile-first if it could be used on mobile
- Accessible by default

Flag anything that requires product/brand taste decision.

### Step 4 — Eng Review (auto-mode)

Run /plan-eng-review mentally. Auto-apply:
- Standard patterns over custom solutions
- Migrations must have rollback
- All endpoints require auth unless explicitly public
- No N+1 queries

Flag anything requiring architectural taste decisions.

### Step 5 — Approval gate

Present all flagged taste decisions together:

```
## Autoplan Review: <plan name>

### Auto-decided
- CEO: [HOLD SCOPE] — trimmed X, Y, Z as out of scope
- Design: [SIMPLIFIED] — reduced to N steps
- Eng: [STANDARD STACK] — using existing patterns

### Your decisions needed (taste calls)

1. [SCOPE] Should we include X in v1 or defer to v2?
   - Option A: include (adds 3 days, makes feature complete)
   - Option B: defer (ship faster, users can request)

2. [DESIGN] Primary navigation: tabs vs sidebar?
   - Option A: tabs (mobile-friendly, simpler)
   - Option B: sidebar (more features visible, desktop UX)

### After your decisions

I'll produce the final reviewed plan and you can start implementing.
```

Wait for user answers, then produce the final plan document.
