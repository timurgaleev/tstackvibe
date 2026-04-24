---
name: design-review
description: |
  Visual design review — evaluates an existing UI for clarity, hierarchy, spacing,
  typography, color, and interaction quality. Catches AI design slop. Use when
  reviewing implemented UI: "design review", "review this UI", "check the design",
  "does this look good".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - AskUserQuestion
  - WebSearch
triggers:
  - design review
  - review this ui
  - check the design
  - does this look good
  - slop check
---

## Design Review

### Step 1 — Get the design

Ask: "Share the UI — screenshot, HTML file path, or URL."

Read the HTML/CSS/JSX if it's in the codebase:

```bash
find . -name "*.tsx" -o -name "*.jsx" -o -name "*.html" | grep -v node_modules | head -10
```

### Step 2 — AI Slop Check (run first)

Common AI-generated design anti-patterns to catch immediately:

- [ ] Placeholder text ("Lorem ipsum", "Your text here", "Coming soon")
- [ ] Generic stock photo placeholders
- [ ] Every element the same size/weight (no hierarchy)
- [ ] Cards for everything regardless of whether content is card-appropriate
- [ ] Gradient on gradient on gradient
- [ ] Shadow on shadow
- [ ] "Made with AI" aesthetic — overly clean, no personality
- [ ] Feature lists that don't connect to user benefit
- [ ] Marketing copy as UI copy ("Revolutionize your workflow")
- [ ] Buttons that say "Click Here" or "Submit"

### Step 3 — Visual hierarchy

- Is there a clear focal point? Does the eye know where to go first?
- Is the primary action the most prominent element?
- Are secondary actions clearly secondary?
- Is text content organized by importance?

### Step 4 — Typography

- Font sizes: at least 3 sizes used? (heading, body, caption)
- Line height: 1.4-1.6 for body text?
- Line length: 45-75 characters per line for readable text?
- Contrast: minimum 4.5:1 for body text?

### Step 5 — Spacing and layout

- Consistent spacing scale? (4px / 8px / 16px / 24px / 32px)
- Alignment: elements aligned to a grid?
- Breathing room: enough whitespace around key elements?
- Visual grouping: related elements visually grouped?

### Step 6 — Color

- Color used for meaning, not decoration?
- Sufficient contrast for all text?
- Color not the only way to communicate state (also use icons/labels)?
- Maximum 3-4 colors in the main palette?

### Step 7 — Interactions

- Interactive elements look interactive? (cursor, hover state)
- Loading states shown?
- Empty states designed?
- Error states designed?

### Output

```
## Design Review: <screen/feature>

### Slop score: X/10 (0 = clean, 10 = all slop)
Slop found: <list>

### Visual hierarchy: CLEAR / MUDDY / BROKEN
<finding>

### Typography: PASS / ISSUES
<finding>

### Spacing: PASS / ISSUES
<finding>

### Color: PASS / ISSUES
<finding>

### Top fixes (in priority order):
1. [CRITICAL] <issue> → <specific fix>
2. [HIGH] <issue> → <specific fix>
3. [MEDIUM] <issue> → <specific fix>

### Overall verdict: SHIP / POLISH FIRST / REDESIGN
```
