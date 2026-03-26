---
name: ship-init
description: This skill should be used when the user asks to "set up ship", "configure ship", "ship init", "initialize ship", "create my profile", "set up my profile", or when using the ship plugin for the first time. Creates a .claude/ship.local.md profile that calibrates all other ship skills.
argument-hint: (no arguments needed)
allowed-tools: AskUserQuestion, Write, Read, Glob, Bash
---

# Ship Init

Create the user's ship profile at `.claude/ship.local.md`. This file calibrates how all other ship skills behave.

## Process

### 1. Check for existing profile

Read `.claude/ship.local.md` in the current project. If it exists, show the current profile and ask whether to update it or start fresh.

### 2. Ask the questions

Ask these one at a time, conversationally. Do not dump all at once.

1. What's your role? (founder, solo dev, tech lead, non-technical founder, etc.)
2. What stage is this project at? (prototype/MVP, early users, scaling, mature)
3. What matters most right now? (speed to market, code quality, security, user experience, cost — pick up to 2)
4. Default review mode: brutal (direct, high bar, assumes you can handle it) or kind (pragmatic, flags only what truly matters at your stage)?
5. Want debrief nudges? Other skills can remind you when a weekly debrief is due. (yes/no — default: no)

### 3. Write the profile

Write `.claude/ship.local.md` with YAML frontmatter and a brief markdown body:

```markdown
---
role: founder
project-stage: mvp
priorities:
  - speed to market
  - user experience
default-review-mode: brutal
always-flag:
  - security
  - data loss
debrief-nudges: false
last-debrief: never
---

# Ship Profile

[One-sentence summary of who this person is and what they care about, written by you based on their answers.]
```

The `always-flag` field defaults to `[security, data loss]`. The `last-debrief` field tracks when `/ship:debrief` was last run. Both can be edited by the user directly.

### 4. Shared repo check

Ask: "Is this repo shared with others, or just you?" (If it's just them, the profile can be committed. If shared, it should stay local.)

If shared (or the user says they'd rather keep it private):
- Check if `.claude/*.local.md` is already in `.gitignore`. If not, append it.
- Briefly confirm: "Added `.claude/*.local.md` to .gitignore — your profile won't be committed."

If solo / user wants to commit it:
- Do nothing. The file will be tracked normally.

### 5. Confirm

Show the user what was written. Tell them they can re-run `/ship:init` anytime or edit the file directly.

## Guidelines

- Keep the conversation warm but efficient. Two minutes max.
- Do not over-explain what the profile is for. The user chose to install this plugin — they get it.
- If the user gives terse answers, work with what you get.
- The profile should be short. Under 15 lines total including frontmatter.
