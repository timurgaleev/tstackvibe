---
name: plan-eng-review
description: |
  Engineering plan review. Validates technical approach, architecture, data model,
  API design, scalability, and implementation risks before writing code. Use when
  asked to "review the technical plan", "eng review", "check the implementation plan".
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - AskUserQuestion
  - WebSearch
triggers:
  - review the technical plan
  - eng review
  - check the implementation plan
  - engineering review
---

## Step 1 — Read the plan

Read the implementation plan completely. If none exists, ask for it.

```bash
# Check for existing plans
find . -name "*.md" | xargs grep -l "plan\|implementation\|architecture" 2>/dev/null | head -10
```

## Step 2 — Evaluate each dimension

### Architecture
- Is the component breakdown right? Are boundaries clean?
- What's the data flow? Are there unnecessary hops?
- Is the system stateless where it can be? Stateful where it must be?
- Will this design make the next feature easy or hard?

### Data model
- Are entities and relationships modeled correctly?
- Are indexes planned for every query pattern?
- Is there a migration strategy with rollback?
- Are nullable columns intentional?

### API design
- Are endpoints RESTful and consistent?
- Is pagination designed for all list endpoints?
- Is error response format consistent?
- Are breaking changes called out?

### Scalability
- Where are the bottlenecks at 10x load?
- What gets slow first: database, API, external calls?
- Is caching planned where appropriate?

### Security
- Authentication and authorization at every endpoint?
- Input validation and output encoding?
- Rate limiting on public-facing APIs?
- No secrets in code?

### Implementation risks
- What's the riskiest part of the plan?
- Are there external dependencies that could block progress?
- What's the fallback if the approach doesn't work?

### Testing strategy
- Unit tests for business logic?
- Integration tests for critical paths?
- What's the manual QA plan?

## Output

```
## Eng Review: <plan name>

### Architecture: APPROVED / CONCERNS / REWORK
<findings>

### Data model: APPROVED / CONCERNS / REWORK
<findings>

### API design: APPROVED / CONCERNS / REWORK
<findings>

### Security: APPROVED / CONCERNS / REWORK
<findings>

### Top risks:
1. [CRITICAL] <risk> → <mitigation>
2. [HIGH] <risk> → <mitigation>
3. [MEDIUM] <risk> → <mitigation>

### Verdict: PROCEED / REVISE FIRST / REDESIGN

**Blocking issues to fix before coding:**
- ...

**Suggested changes:**
- ...
```
