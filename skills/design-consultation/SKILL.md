---
name: design-consultation
description: |
  Design consultation — structured conversation to clarify design direction before
  building UI. Asks about users, context, constraints, and aesthetic direction.
  Use when starting a UI feature: "design consultation", "help me design this",
  "design direction", before building any significant UI.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - AskUserQuestion
  - WebSearch
triggers:
  - design consultation
  - help me design this
  - design direction
  - what should this look like
---

## Design Consultation

### Step 1 — Understand the user

Ask one at a time:

> "Who is the primary user of this UI? Age range, technical level, primary device (mobile/desktop/both)?"

> "What's the main task they're trying to complete in 10 words or less?"

> "What are they feeling when they arrive at this screen? (stressed, excited, confused, in a hurry?)"

### Step 2 — Understand the context

> "Is this part of an existing product? Share the design system or brand colors if you have them."

> "What does success look like for the user on this screen? What's the ONE action they must be able to do?"

> "What's the most common mistake users make on similar screens that you want to prevent?"

### Step 3 — Aesthetic direction

> "Pick 3 words that describe the feel you want: e.g., professional, playful, minimal, bold, trustworthy, urgent."

> "Any products or UIs you love as reference? What do you love about them?"

> "Any hard constraints: specific colors, accessibility level (AA/AAA), RTL support needed?"

### Step 4 — Synthesize

After answers, present a design brief:

```
## Design Brief: <feature name>

**User:** <description>
**Primary task:** <one sentence>
**User feeling:** <emotional context>
**Aesthetic:** <3 words>
**Key constraints:** <list>
**Success metric:** <how we know the design works>

**Recommended approach:**
<2-3 sentences on the overall direction>

**References:** <any mentioned>
```

Ask: "Does this capture it? Want me to now build a mockup with /design-html or a detailed spec?"
