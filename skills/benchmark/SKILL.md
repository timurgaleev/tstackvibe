---
name: benchmark
description: |
  Performance benchmarking. Measures build size, test run times, and code-level
  performance metrics. Establishes baselines and detects regressions between
  commits. Use when: "performance", "benchmark", "bundle size", "how fast is X",
  "performance regression", "is this slower".
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
  - AskUserQuestion
triggers:
  - performance benchmark
  - check bundle size
  - detect performance regression
  - how fast is this
  - is this slower
---

## Benchmark Workflow

### Step 1 — Define what to benchmark

Ask if not clear:
> "What to benchmark? Options: (1) Build size/bundle, (2) Test suite speed, (3) Specific function/endpoint, (4) All of the above"

### Step 2 — Run benchmarks

**Build size**
```bash
# JS/TS
npm run build 2>/dev/null
du -sh dist/ build/ .next/ out/ 2>/dev/null
find dist/ build/ .next/ -name "*.js" 2>/dev/null | xargs wc -c 2>/dev/null | sort -rn | head -10

# Check for large dependencies
cat package.json | python3 -c "import json,sys; d=json.load(sys.stdin); print('\n'.join(sorted(d.get('dependencies',{}).keys())))" 2>/dev/null
```

**Test suite speed**
```bash
time npm test -- --passWithNoTests 2>/dev/null
time python -m pytest 2>/dev/null
time go test ./... 2>/dev/null
```

**Specific function benchmark (JS/TS)**
```bash
# If there's a benchmark file
find . -name "*.bench.ts" -o -name "*.bench.js" -o -name "benchmark.js" 2>/dev/null | grep -v node_modules
npm run bench 2>/dev/null || npx vitest bench 2>/dev/null
```

**Specific function benchmark (Python)**
```bash
python -m pytest --benchmark-only 2>/dev/null
```

### Step 3 — Compare to baseline

```bash
# Check if prior benchmark exists
cat .tstackvibe/benchmark-baseline.md 2>/dev/null
```

If no baseline: "No baseline found. I'll save these as the new baseline."
If baseline exists: compare each metric and flag regressions (>10% worse).

### Step 4 — Output

```
## Benchmark: <project>
**Date:** YYYY-MM-DD
**Commit:** <hash>

### Build size
| Output | Size | Delta |
|--------|------|-------|
| dist/ | Xmb | +Y% vs baseline |

### Test suite
| Suite | Time | Delta |
|-------|------|-------|
| all tests | Xs | +Y% vs baseline |

### Key regressions (>10% slower)
- <what> — was Xs, now Xs (+Y%) → NEEDS INVESTIGATION

### Key improvements
- <what> — was Xs, now Xs (-Y%)

### Verdict: PASS / REGRESSION / MAJOR REGRESSION
```

Save to `.tstackvibe/benchmark-<date>.md`.

If baseline didn't exist: save as `.tstackvibe/benchmark-baseline.md`.

Offer: "Want me to investigate any regressions?"
