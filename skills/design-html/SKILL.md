---
name: design-html
description: |
  Generate a high-quality HTML mockup of a UI — a single self-contained HTML file
  with inline CSS and realistic content. No placeholders. Use when asked to
  "mock this up", "show me what this could look like", "design-html", "build a mockup".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - AskUserQuestion
  - WebSearch
triggers:
  - mock this up
  - show me what this could look like
  - design html
  - build a mockup
  - design this ui
---

## HTML Mockup Workflow

### Step 1 — Gather requirements

Ask if not already clear:
> "What's the feature/screen? Who's the user? What's the primary action?"

Read any existing design consultation output or design brief.

### Step 2 — Plan the mockup

Before writing HTML, define:
- Layout: full page / component / modal?
- Primary action: what's the hero element?
- Content: what real content goes here? (no Lorem ipsum)
- States needed: empty, loaded, error, loading?

### Step 3 — Write the HTML

Requirements for the output:
- **Single file** — inline all CSS, no external dependencies (except system fonts)
- **No Lorem ipsum** — use realistic, domain-appropriate content
- **Realistic data** — real names, real numbers, real dates
- **No placeholders** — every element has real content
- **Responsive** — works on mobile and desktop
- **Accessible** — semantic HTML, proper labels, contrast ≥4.5:1
- **Interactive feel** — hover states on interactive elements (CSS only)
- **Pixel-clean** — consistent spacing scale (4/8/16/24/32px)

### Step 4 — Anti-patterns to avoid

- Generic "hero section" with big gradient text
- Card grid for everything
- Marketing copy in UI
- Buttons that say "Get Started" or "Learn More" without context
- Shadow stacking
- Icon + text that says the same thing twice
- Empty `<div>` spacers

### Step 5 — Save and open

Save to `mockups/<feature-name>-<date>.html`

```bash
open mockups/<feature-name>-<date>.html 2>/dev/null || echo "Open the file in your browser"
```

Offer: "Want me to run /design-review on this mockup? Or should we refine it first?"
