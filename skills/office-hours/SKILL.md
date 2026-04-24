---
name: office-hours
description: |
  Product/idea brainstorming session. Two modes: Startup mode (6 forcing questions
  that expose demand reality and narrow wedge) and Builder mode (design thinking for
  side projects, open source, learning). Saves a design doc. Use when asked to
  "brainstorm this", "I have an idea", "help me think through this", "office hours".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Write
  - Edit
  - AskUserQuestion
  - WebSearch
triggers:
  - brainstorm this
  - is this worth building
  - help me think through
  - office hours
  - i have an idea
---

## Session Start

Ask the user which mode:

> "Two modes: **Startup** (real demand, who pays, narrow wedge) or **Builder** (design thinking, explore freely). Which fits?"

---

## Startup Mode — 6 Forcing Questions

Ask one at a time. Wait for a full answer before the next.

**1. Demand reality**
> "Can you name 3 specific people (not personas) who would pay for this today? What's their name, role, and why they need it?"

**2. Status quo**
> "What do those people do right now instead of using your solution? How painful is the current workaround on a scale 1-10?"

**3. Desperate specificity**
> "Who is desperate enough to pay for an embarrassingly bad v1? Describe the 10 most desperate users in the world."

**4. Narrowest wedge**
> "What's the smallest version of this that delivers real value? If you had to ship in 2 weeks, what gets cut?"

**5. Observation over assumption**
> "Have you watched a real user struggle with the problem you're solving? What surprised you?"

**6. Future-fit**
> "Why will this be a $10M+ business in 5 years and not a feature that a big company copies in 6 months?"

### After all 6 answers

Synthesize into a design doc. Save it.

```
## Synthesis

**The real problem:** <one sentence>
**Target user:** <specific, not a persona>
**V1 in 2 weeks:** <concrete scope>
**Why now:** <urgency>
**Biggest risk:** <what could kill this>
**Recommended next step:** <single action>
```

---

## Builder Mode — Design Thinking

**1. Understand**
> "What are you trying to build and why? What problem does it solve for you personally?"

**2. Explore**
> "What's the most interesting version of this? Ignore constraints for a moment."

**3. Define**
> "Who else would use this? What would make it feel magical vs. just functional?"

**4. Prototype thinking**
> "What's the smallest experiment you could run in a day to validate the core idea?"

**5. Plan**
> "What's the first thing to build? What can be deferred?"

### After discussion

Offer to save a design doc to `docs/design-<topic>-<date>.md`.

---

## After This Session

Suggest: "Want to run /plan-ceo-review to stress-test scope, or /plan-eng-review to check the implementation plan?"
