---
name: health
description: |
  Code quality dashboard. Runs all available checks (type checker, linter, tests,
  dead code, security), computes a weighted composite 0-10 score, and surfaces
  the most impactful improvements. Use when asked to "health check", "code quality",
  "how healthy is the codebase", "run all checks", "quality score".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - code health check
  - quality dashboard
  - how healthy is codebase
  - run all checks
  - quality score
---

## Health Check Workflow

### Step 1 — Detect available tools

```bash
# Detect project type and tools
ls package.json tsconfig.json Cargo.toml go.mod pyproject.toml requirements.txt 2>/dev/null
cat package.json 2>/dev/null | grep -E '"scripts"' -A 20 | head -25
```

### Step 2 — Run all checks

Run each tool that's available. Capture output and exit codes.

**TypeScript / JavaScript**
```bash
# Type checking
npx tsc --noEmit 2>&1 | tail -20

# Linting
npx eslint . --max-warnings 0 2>&1 | tail -20

# Tests with coverage
npm test -- --coverage --passWithNoTests 2>&1 | tail -30

# Unused exports (if ts-prune available)
npx ts-prune 2>/dev/null | head -20
```

**Python**
```bash
python -m mypy . 2>&1 | tail -20
python -m ruff check . 2>&1 | tail -20
python -m pytest --tb=short 2>&1 | tail -30
```

**Go**
```bash
go vet ./... 2>&1
go test ./... 2>&1 | tail -20
staticcheck ./... 2>/dev/null | tail -10
```

**General**
```bash
# Shell scripts
find . -name "*.sh" | grep -v node_modules | xargs shellcheck 2>/dev/null | tail -20

# Secrets scan
grep -rn "API_KEY\|SECRET\|PASSWORD" --include="*.ts" --include="*.js" --include="*.py" \
  | grep -v node_modules | grep -v ".env.example" | grep -v "process.env\|os.environ" | head -10

# Dead code (rough)
# TODO files
find . -name "*.ts" -o -name "*.js" -o -name "*.py" | xargs grep -l "TODO\|FIXME\|HACK\|XXX" \
  2>/dev/null | grep -v node_modules | head -20
```

### Step 3 — Score each dimension

| Dimension | Weight | Score | Notes |
|-----------|--------|-------|-------|
| Type safety | 25% | 0-10 | Errors = 0, warnings only = 7, clean = 10 |
| Lint | 20% | 0-10 | Errors = 0, warnings = 5, clean = 10 |
| Tests passing | 25% | 0-10 | Failures = 0, passing = 5-10 based on coverage |
| Test coverage | 15% | 0-10 | <50% = 3, 50-70% = 5, 70-85% = 7, 85%+ = 10 |
| Security | 15% | 0-10 | Exposed secrets = 0, issues found = 3-7, clean = 10 |

**Composite score = weighted average**

### Step 4 — Output

```
## Health Report: <project name>
**Date:** YYYY-MM-DD

### Scores
| Check | Score | Status |
|-------|-------|--------|
| Type safety | X/10 | PASS / WARN / FAIL |
| Lint | X/10 | PASS / WARN / FAIL |
| Tests | X/10 | PASS / WARN / FAIL |
| Coverage | X/10 | PASS / WARN / FAIL |
| Security | X/10 | PASS / WARN / FAIL |
| **TOTAL** | **X/10** | |

### Critical issues (fix now)
- ...

### Top improvements by impact
1. [+X pts] Fix <issue> — <how>
2. [+X pts] Fix <issue> — <how>
3. [+X pts] Fix <issue> — <how>

### Trend (if prior report exists)
Previous: X/10 → Today: X/10 (↑ / ↓ / →)
```

Save report to `.tstackvibe/health-<date>.md` for trend tracking.

Offer: "Want me to fix the critical issues now?"
