---
name: canary
description: |
  Canary deploy health check. Monitors error rates, latency, and key metrics after
  a deployment to verify the release is healthy before full rollout. Use when asked
  to "check the canary", "is the deploy healthy", "verify the release", "canary check".
allowed-tools:
  - Bash
  - Read
  - AskUserQuestion
  - WebSearch
triggers:
  - check the canary
  - is the deploy healthy
  - verify the release
  - canary check
---

## Canary Check Workflow

### Step 1 — Confirm context

Ask if not clear:
> "What was deployed? What's the canary URL/environment? What % of traffic is on canary?"

### Step 2 — Baseline metrics

Ask for or check:
- Error rate before deploy (baseline)
- Latency p50/p95/p99 before deploy
- Key business metric baseline (conversion, throughput, etc.)

### Step 3 — Current canary metrics

```bash
# Check logs for errors
# Check monitoring/alerting systems
# Look for 5xx errors
```

Ask for the monitoring URL or credentials if needed.

### Step 4 — Compare

For each metric:
- Error rate: canary vs baseline (alert if >2x baseline)
- Latency p99: alert if >1.5x baseline
- Business metric: alert if >5% regression

### Step 5 — Check logs for new errors

Look for:
- New error types not seen before deploy
- Stack traces from new code paths
- Database errors or timeouts
- External service errors

### Step 6 — Verdict

```
## Canary Health: <deploy description>

**Deploy:** <version/commit>
**Traffic:** X% on canary
**Duration:** X minutes since deploy

### Metrics
| Metric | Baseline | Canary | Delta | Status |
|--------|----------|--------|-------|--------|
| Error rate | X% | Y% | +Z% | GREEN/YELLOW/RED |
| Latency p99 | Xms | Yms | +Z% | GREEN/YELLOW/RED |
| ... | | | | |

### New errors in logs
- None / List of new errors

### Verdict: PROMOTE / HOLD / ROLLBACK

**Reason:** <one sentence>
```

If RED: "I recommend rollback. Run: `git revert HEAD` or trigger rollback in your deploy system."
