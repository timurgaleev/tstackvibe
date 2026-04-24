---
name: plan-ceo-review
description: |
  CEO/founder-mode plan review. Challenge premises, find the 10-star product, rethink
  scope. Four modes: EXPAND (dream big), SELECTIVE (hold scope + cherry-pick wins),
  HOLD (maximum rigor), REDUCE (strip to essentials). Use when asked to "think bigger",
  "expand scope", "strategy review", "rethink this", "is this ambitious enough".
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - AskUserQuestion
  - WebSearch
triggers:
  - think bigger
  - expand scope
  - strategy review
  - rethink this plan
  - is this ambitious enough
---

## Step 1 — Read the plan

If a plan file exists, read it completely. Otherwise ask:
> "Paste the plan, or describe what you're building in a few sentences."

## Step 2 — Choose mode

Ask (or infer from context):

> "Which mode?
> 1. **EXPAND** — Dream bigger, find what's missing
> 2. **SELECTIVE** — Hold scope, cherry-pick the best expansions
> 3. **HOLD** — Maximum rigor, challenge every assumption
> 4. **REDUCE** — Strip to essentials, find the 2-week MVP"

---

## EXPAND Mode

Challenge every scope boundary. Ask:

- "What's the 10-star version of this? What would make users evangelize it?"
- "What adjacent problem do your users have that you're leaving on the table?"
- "If you had 10x the budget and 3x the time, what would you add?"
- "What would make this defensible in 3 years — data moat, network effects, switching costs?"
- "Who are you NOT building for that you should be?"

Output: List of 3-5 expansion ideas with impact/effort rating.

---

## SELECTIVE Mode

Hold current scope. Find only the additions that make the core dramatically better.

- Identify the core value proposition (one sentence)
- For each proposed feature: does it strengthen or dilute the core?
- Cherry-pick only features that are load-bearing for the core

Output: Keep/cut list with reasoning.

---

## HOLD Mode

Attack every assumption:

- "Why does this problem need a new solution vs. using existing tools?"
- "What's the hardest thing about this plan that you're not saying out loud?"
- "What happens if your key assumption is wrong?"
- "What does the competition do that you're ignoring?"
- "Where is the plan optimistic? What's the realistic timeline?"

Output: List of risks ranked by severity, with mitigation for each.

---

## REDUCE Mode

Strip to minimum:

- "If you had to ship in 2 weeks, what's the ONE thing that delivers value?"
- "What's the riskiest assumption? Test that first."
- "What can wait until v2 without losing the core value?"

Output: 2-week MVP definition with explicit cut list.

---

## Final Output

```
## CEO Review: <plan name>

**Mode:** EXPAND / SELECTIVE / HOLD / REDUCE

**Core finding:** <one sentence verdict>

**Top 3 issues/opportunities:**
1. ...
2. ...
3. ...

**Recommended next action:** <single most important thing to do>
```
