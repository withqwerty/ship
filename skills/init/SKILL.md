---
name: ship-init
description: Use this skill when the user wants to set up, configure, or initialize the ship plugin, create or update their ship profile, change their review mode, update their product brief, or edit .claude/ship.local.md. Trigger on "ship init", "set up ship", "configure ship", "create my profile", "update my profile", "change review mode to brutal/kind", or when any other ship skill detects that .claude/ship.local.md does not exist. Also use when the user says they've changed roles, shifted priorities, or moved to a new project stage.
argument-hint: (no arguments needed)
allowed-tools: AskUserQuestion, Write, Read, Glob, Bash
---

# Ship Init

Create the user's ship profile at `.claude/ship.local.md`. This file is two things: a settings file that calibrates ship skills, and a living product brief that every skill reads to understand what matters.

## Process

### 1. Check for existing profile

Read `.claude/ship.local.md` in the current project. If it exists, show the current profile and ask whether to update it or start fresh.

### 2. Ask the questions

Ask these one at a time, conversationally. Do not dump all at once.

**About you:**
1. What's your role? (founder, solo dev, tech lead, non-technical founder, etc.)
2. What stage is this project at? (prototype/MVP, early users, scaling, mature)

**About the product:**
3. In one sentence, what does this product do and who is it for?
4. What are the 1-2 things that would actually move the needle right now? (Not engineering tasks — product/business outcomes. e.g. "get first 100 users", "reduce churn", "launch the paid tier", "prove the core loop works")

**Ship settings:**
5. What matters most in terms of how you build? (speed to market, code quality, security, user experience, cost — pick up to 2)
6. Default review mode: brutal or kind?
7. Want debrief nudges? (yes/no — default: no)

### 3. Write the profile

Write `.claude/ship.local.md` with YAML frontmatter and a markdown body:

```markdown
---
role: founder
project-stage: mvp
default-review-mode: brutal
build-priorities:
  - speed to market
  - user experience
always-flag:
  - security
  - data loss
debrief-nudges: false
last-debrief: never
---

# [Product Name]

[One sentence: what it does and who it's for.]

## What moves the needle

- [Concrete outcome 1 — framed as a result, not a task]
- [Concrete outcome 2]

## Current reality

[2-3 sentences: where the product actually is right now. What's working, what's not, what's unknown. Written by you based on the conversation and a glance at the codebase. Be honest — this is the "you are here" marker.]
```

The markdown body is the product brief. It gets updated by `/ship:debrief` as priorities evolve. Every ship skill reads it to understand not just who the user is, but what they're building and what matters for it right now.

The `always-flag` field defaults to `[security, data loss]`. The `last-debrief` field tracks when `/ship:debrief` was last run.

### 4. Shared repo check

Ask: "Is this repo shared with others, or just you?"

If shared (or user prefers private):
- Check if `.claude/*.local.md` is already in `.gitignore`. If not, append it.
- Briefly confirm: "Added `.claude/*.local.md` to .gitignore — your profile won't be committed."

If solo / user wants to commit it:
- Do nothing. The file will be tracked normally.

### 5. Confirm

Show the user what was written. Tell them they can re-run `/ship:init` anytime, or edit the file directly. Mention that `/ship:debrief` will keep the product brief evolving.

## Guidelines

- Keep the conversation warm but efficient. Three minutes max.
- The product questions (3 and 4) are the most important. Push for specificity. "Grow the business" is not a needle-mover — "get 10 paying customers" is.
- Glance at the codebase to write the "Current reality" section. A quick look at the project structure, README, and recent commits is enough to write 2-3 honest sentences.
- Do not over-explain what the profile is for.
- The product brief should read like Jason Fried wrote it — clear, concise, opinionated about what matters. No corporate language, no hedging.
