---
name: devex-review
description: |
  Developer experience review — evaluates the local dev setup, onboarding experience,
  tooling, CI/CD, and documentation from a new developer's perspective. Use when
  asked to "devex review", "check the dev setup", "review onboarding", "is this
  easy to set up?".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - AskUserQuestion
triggers:
  - devex review
  - check the dev setup
  - review onboarding
  - developer experience
  - is this easy to set up
---

## DevEx Review Workflow

### Step 1 — Read the onboarding docs

```bash
cat README.md 2>/dev/null
cat docs/CONTRIBUTING.md 2>/dev/null || cat CONTRIBUTING.md 2>/dev/null
cat docs/development.md 2>/dev/null
```

### Step 2 — Evaluate local setup

**Getting started**
- Can a new dev get running with ≤5 commands?
- Are all prerequisites listed with install links?
- Is there a one-command setup script?
- Does it work without `.env` variables? Or is there a `.env.example`?

**Running the project**
```bash
cat package.json | grep -E '"dev"|"start"|"build"'
ls Makefile docker-compose.yml 2>/dev/null
```
- Single command to start the dev server?
- Hot reload configured?
- Is the dev database seeded with realistic data?

**Running tests**
- Single command to run all tests?
- Fast enough for the inner loop? (goal: <30s for unit tests)
- Clear output when tests fail?

### Step 3 — Evaluate CI/CD

```bash
ls .github/workflows/ 2>/dev/null
cat .github/workflows/*.yml 2>/dev/null | head -50
```

- CI runs on every PR?
- CI fails fast (most common failures first)?
- Reasonable CI time? (goal: <10 min for PR checks)
- Deployment automated?
- Is staging environment documented?

### Step 4 — Evaluate tooling

- Linter configured and running in CI?
- Formatter auto-applied on commit?
- TypeScript strict mode? (for TS projects)
- Pre-commit hooks configured?

### Step 5 — Evaluate documentation

- Architecture overview exists?
- Key decisions documented (ADRs)?
- API documented?
- Common gotchas documented?
- "Why is X done this way?" answers findable?

### Step 6 — New developer simulation

Mentally simulate a new developer's first day:
1. Clone the repo
2. Follow README to set up
3. Run the app
4. Make a small change
5. Run tests
6. Submit a PR

Where do they get stuck?

### Output

```
## DevEx Review: <project name>

### Getting started: X/10
<findings and specific fixes>

### Local development: X/10
<findings>

### Testing: X/10
<findings>

### CI/CD: X/10
<findings>

### Documentation: X/10
<findings>

### Overall DevEx score: X/10

### Top improvements (priority order):
1. [HIGH] <issue> → <specific fix> (<estimated effort>)
2. [MEDIUM] <issue> → <specific fix>
3. [LOW] <issue> → <specific fix>
```
