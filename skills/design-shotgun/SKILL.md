---
name: design-shotgun
description: |
  Generate 3 distinct design variants for a UI feature — different approaches,
  different aesthetics, different interaction models. Compare them to choose
  a direction. Use when: "show me design options", "explore designs", "design variants",
  "I don't like how this looks", "visual brainstorm".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - explore design variants
  - show me design options
  - visual design brainstorm
  - design options
  - i dont like how this looks
---

## Design Shotgun Workflow

### Step 1 — Understand the target

Ask if not clear:
> "What UI element or screen are we designing? Who is the primary user? What's the main action?"

Read any existing code or design docs for context.

### Step 2 — Define 3 distinct directions

Before writing any HTML, define 3 meaningfully different approaches:

**Direction A** — Conservative/Professional
- Who it's for, what aesthetic, key interaction model

**Direction B** — Bold/Modern
- Who it's for, what aesthetic, key interaction model

**Direction C** — Minimal/Focused
- Who it's for, what aesthetic, key interaction model

Ask: "Confirm these 3 directions, or suggest different angles?"

### Step 3 — Generate all 3 variants

For each variant, create a single self-contained HTML file:

Requirements:
- No Lorem ipsum — realistic domain content
- Self-contained (inline CSS, no external deps except system fonts)
- Labeled clearly with the direction name
- Mobile-responsive

Save as:
- `mockups/shotgun-A-conservative-<date>.html`
- `mockups/shotgun-B-bold-<date>.html`
- `mockups/shotgun-C-minimal-<date>.html`

```bash
mkdir -p mockups
open mockups/shotgun-A-*.html 2>/dev/null || echo "Open the files in your browser to compare"
```

### Step 4 — Present comparison

After generating:

```
## Design Variants: <feature name>

**Variant A — Conservative**
- Aesthetic: <description>
- Best for: <user/context>
- Strength: <what it does well>
- Weakness: <tradeoff>

**Variant B — Bold**
- Aesthetic: <description>
- Best for: <user/context>
- Strength: <what it does well>
- Weakness: <tradeoff>

**Variant C — Minimal**
- Aesthetic: <description>
- Best for: <user/context>
- Strength: <what it does well>
- Weakness: <tradeoff>

**My recommendation:** <which one and why>
```

### Step 5 — Iterate

Ask: "Which direction resonates? Or want to take elements from multiple variants?"

After picking a direction, offer to refine it with `/design-html` for a production-quality mockup.
