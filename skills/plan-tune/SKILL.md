---
name: plan-tune
description: |
  Tune how skills ask questions. Set per-question preferences (never ask / always ask /
  ask only for irreversible actions). Review your interaction style. Reduce interruptions
  for experienced users. Use when: "tune questions", "stop asking me that",
  "too many questions", "show my profile", "I want fewer confirmations".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - AskUserQuestion
triggers:
  - tune questions
  - stop asking me that
  - too many questions
  - show my profile
  - i want fewer confirmations
  - developer profile
---

## Plan Tune

### What this does

Configures how skills behave — specifically which questions they ask and how much
they pause for confirmation. Preferences are saved to `.tstackvibe/tune.json`.

### Step 1 — Show current profile

```bash
cat .tstackvibe/tune.json 2>/dev/null || echo "No profile saved yet — using defaults"
```

Defaults:
```json
{
  "confirm_before_merge": true,
  "confirm_before_deploy": true,
  "confirm_version_bump": true,
  "confirm_before_write_files": false,
  "ask_for_test_command": true,
  "ask_for_deploy_url": true,
  "proactive_skill_suggestions": true,
  "verbosity": "default"
}
```

### Step 2 — Tune conversation

Ask what the user wants to change:

> "What's bothering you? I can:
> - Stop asking for certain confirmations
> - Reduce verbosity (terse mode)
> - Turn off proactive skill suggestions
> - Set defaults for common values (test command, deploy URL, etc.)
> Just tell me what you want."

### Common requests

**"Stop asking before every file write"**
```json
{ "confirm_before_write_files": false }
```

**"Stop asking me to confirm before merging"**
```json
{ "confirm_before_merge": false }
```

**"Less verbose output"**
```json
{ "verbosity": "terse" }
```

**"Stop suggesting skills I didn't ask for"**
```json
{ "proactive_skill_suggestions": false }
```

**"Remember my test command"**
```json
{ "default_test_command": "npm test" }
```

**"Remember my deploy URL"**
```json
{ "default_deploy_url": "https://your-app.com" }
```

### Step 3 — Save preferences

```bash
mkdir -p .tstackvibe
# Write updated tune.json
```

Confirm: "Saved. These preferences apply to all tstackvibe skills in this project."

### Step 4 — Show updated profile

Display the new profile and explain what changed.

Note: Skills read `.tstackvibe/tune.json` at runtime. Changes take effect immediately in the next skill invocation.
