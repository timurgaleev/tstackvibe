---
name: plan-design-review
description: |
  Design plan review. Evaluates UX, information architecture, user flows, visual
  hierarchy, and interaction patterns before building UI. Use when asked to
  "design review", "review the UX plan", "check the design", "review user flows".
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - AskUserQuestion
  - WebSearch
triggers:
  - design review
  - review the ux plan
  - check the design
  - review user flows
  - ux review
---

## Step 1 — Understand the context

```bash
# Look for existing design docs, mockups, or specs
find . -name "*.md" | xargs grep -l "design\|ux\|user\|flow" 2>/dev/null | head -10
ls docs/ 2>/dev/null
```

Ask if needed: "Share the design doc, mockup description, or user flow you want reviewed."

## Step 2 — Evaluate each dimension

### User mental model
- Does the UI match how users think about the problem (not how the database is structured)?
- Is terminology consistent with what users already know?
- Will a new user understand the primary action within 5 seconds?

### Information architecture
- Is content organized by user task, not system structure?
- Is navigation hierarchy correct? (max 3 levels deep)
- Is the most important content most prominent?

### User flows
- Can the primary task be completed in the fewest possible steps?
- Are error states handled gracefully with clear recovery paths?
- Are confirmation dialogs used only for destructive actions?
- Is there a clear success state?

### Interaction patterns
- Are interactive elements (buttons, links, forms) obvious?
- Is loading/pending state communicated?
- Are form validations inline and immediate?
- Are destructive actions reversible or double-confirmed?

### Accessibility
- Color contrast ratio minimum 4.5:1?
- Touch targets minimum 44×44px?
- Keyboard navigable?
- Screen reader friendly?

### Mobile/responsive
- Does it work on mobile? Is touch-first or desktop-first correct for this use case?
- Are tables/data displays mobile-friendly?

### AI slop check (if AI-generated designs)
- Placeholder text instead of real examples?
- Generic stock photo suggestions?
- "Lorem ipsum" content anywhere?
- Feature suggestions not tied to user need?

## Output

```
## Design Review: <feature/page name>

### User flow: CLEAR / FRICTION / REDESIGN
<findings>

### Information architecture: GOOD / ISSUES
<findings>

### Interactions: GOOD / ISSUES
<findings>

### Accessibility: PASS / ISSUES
<findings>

### Top issues:
1. [CRITICAL] <issue> → <recommendation>
2. [HIGH] <issue> → <recommendation>
3. [MEDIUM] <issue> → <recommendation>

### Verdict: PROCEED / REVISE FIRST / REDESIGN
```
