---
name: guard
description: |
  Code guard — audits a file or module to define its invariants and contracts,
  then monitors changes to catch violations. Use when you want to protect critical
  code from accidental breakage: "guard this file", "protect this module", "audit invariants".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Write
  - AskUserQuestion
triggers:
  - guard this file
  - protect this module
  - audit invariants
  - define contracts
---

## Guard Workflow

### Step 1 — Read the code

Read the target file/module completely:

```bash
# Ask for the file path if not given
```

### Step 2 — Extract invariants

For each public function/class/API, define:

**Preconditions** — what must be true before calling
**Postconditions** — what must be true after calling
**Invariants** — what must always be true

### Step 3 — Identify fragile points

- Functions that silently fail
- Shared state that could be corrupted
- External dependencies with no fallback
- Functions where argument order matters

### Step 4 — Write the guard document

Save to `docs/guards/<module-name>.md`:

```markdown
# Guard: <module name>

## Purpose
<one sentence — what this module does>

## Invariants (must always be true)
1. <invariant>
2. <invariant>

## Contracts

### <function name>
- **Preconditions:** <what caller must ensure>
- **Postconditions:** <what this guarantees>
- **Side effects:** <what changes in the world>
- **Throws:** <error conditions>

## Fragile points
- <line/function>: <why it's fragile and how to handle>

## Do not change without
- [ ] Running test suite
- [ ] Checking <dependency>
- [ ] Notifying <team/person>

## Last audited: YYYY-MM-DD
```

### Step 5 — Suggest tests

For each fragile point, suggest a test that would catch breakage. Offer to write them.
