---
name: plan-devex-review
description: |
  Interactive developer experience plan review for developer-facing products
  (APIs, CLIs, SDKs, libraries, platforms, docs). Three modes: EXPANSION
  (competitive advantage), POLISH (bulletproof every touchpoint), TRIAGE
  (critical gaps only). Use when asked to "DX review", "API design review",
  "developer experience audit", "devex plan review".
allowed-tools:
  - Read
  - Edit
  - Grep
  - Glob
  - Bash
  - AskUserQuestion
  - WebSearch
triggers:
  - dx review
  - developer experience audit
  - devex plan review
  - api design review
  - onboarding review
---

## Plan DevEx Review

### Step 1 — Read the plan

Read the plan completely. If none exists, ask for it or describe the developer-facing product.

### Step 2 — Choose mode

> "Which mode?
> 1. **EXPANSION** — Find competitive advantages and table-stakes features missing
> 2. **POLISH** — Bulletproof every developer touchpoint end-to-end
> 3. **TRIAGE** — Identify only the critical gaps blocking adoption"

---

## EXPANSION Mode

Find what's missing relative to best-in-class developer products:

**Onboarding (first 5 minutes)**
- Can a developer go from zero to first success in ≤5 minutes?
- Is there a "Hello World" that works first try?
- Is there a working quickstart with copy-paste commands?

**Documentation**
- Getting started guide?
- API reference (auto-generated + human-readable)?
- Conceptual guides ("how X works")?
- Cookbook / examples for common patterns?
- Changelog with migration guides?

**Error experience**
- Do errors say what went wrong AND how to fix it?
- Are errors searchable (does Google return your docs)?
- Are error codes consistent and documented?

**Tooling**
- CLI / SDK available?
- Type definitions / autocomplete?
- Local development mode?
- Sandbox / test environment?

**Community & support**
- Where do developers ask questions?
- Is there a status page?
- Are there office hours, a Discord, or GitHub Discussions?

Output: Gap list with competitive importance rating (table-stakes / differentiator / nice-to-have).

---

## POLISH Mode

Trace the entire developer journey, find friction at every step:

1. **Discovery** — How do developers find out about this? Is the value prop clear in ≤10 seconds?
2. **Signup / access** — How hard is it to get an API key or access? Is there a free tier or trial?
3. **First integration** — Does the quickstart actually work? Test every command copy-paste.
4. **Real-world use** — What happens on the second day? Third? A month in?
5. **Debugging** — When something goes wrong, how does the developer figure out why?
6. **Scaling** — Are there gotchas at scale that aren't documented?
7. **Migration** — If the API changes, how painful is updating?

For each step: friction score (1-10), specific friction points, specific fixes.

---

## TRIAGE Mode

Focus only on adoption blockers:

- Things that prevent a developer from completing the quickstart
- Missing table-stakes features that competitors have
- Error messages with no path to resolution
- Bugs in documentation examples

Output: Ordered list with estimated adoption impact.

---

## Final Output

```
## DevEx Plan Review: <product name>

**Mode:** EXPANSION / POLISH / TRIAGE

### Critical gaps (block adoption)
1. <gap> → <specific fix> (<effort>)

### High-value improvements
1. <improvement> → <specific fix>

### Table-stakes missing (competitors have this)
1. <feature> → <why it matters>

### Strengths (keep these)
- ...

### Recommended priority order
1. ...

### Verdict: READY / NEEDS WORK / RETHINK FUNDAMENTALS
```
