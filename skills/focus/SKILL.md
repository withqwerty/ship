---
name: ship-focus
description: This skill should be used when the user is mid-build and drifting, scope is creeping, new requirements have surfaced, or focus is lost. Triggers on "ship focus", "am I scope creeping", "what should I cut", "I'm losing focus", "this is getting bigger than expected", "refocus", "scope check", or when the user expresses frustration about a task growing beyond its original intent.
argument-hint: <what you originally set out to build (optional — will infer from context)>
allowed-tools: Read, Glob, Grep, Bash, AskUserQuestion
---

# Ship Focus

When the thing has grown beyond what you set out to do, this skill looks at what's actually changed, compares it to the original intent, and tells you what to cut and what to finish.

## Process

### 1. Establish the original intent

Determine what the user originally set out to build, from one of these sources (in priority order):

1. The user's argument to this command
2. Recent conversation history (look for the initial task description or `/ship:think` output)
3. Ask: "In one sentence, what did you sit down to build?"

Capture this as a single sentence. This is the anchor everything gets measured against.

### 2. Read the profile

Check `.claude/ship.local.md` for context. Read the product brief — the "What moves the needle" section is your reference for what matters. If the drift is toward something that serves a needle-mover, it might be worth keeping. If it's away from all of them, cut it.

### 3. Examine what's actually changed

Run `git diff` (staged + unstaged) and `git diff --stat` to understand the scope of changes. Also check `git status` for new untracked files.

Read the changed files to understand what work has been done. Categorise each change as:

- **Core** — directly serves the original intent
- **Adjacent** — related but wasn't part of the plan (refactors, improvements spotted along the way)
- **Drift** — new scope that crept in (new features, new patterns, "while I'm here" changes)

### 4. Deliver the verdict

Output a short, direct assessment. Format:

**What you said you'd build:** [one sentence from step 1]

**What you've actually built:** [one sentence summary of the diff]

**Core** (finish these):
- [file/change]: [why it matters]

**Cut or defer** (these aren't what you sat down to do):
- [file/change]: [what it is and why it can wait]

**If there's drift**, be specific about what to revert or stash:
> `src/utils/auth.ts` — You started refactoring the auth helpers. This isn't what you're shipping. `git stash` this or revert it.

**If there's no drift**, say so:
> You're on track. Everything here serves the original goal. Keep going.

### 5. The nudge

End with one sentence of guidance calibrated to the situation:

- If they're deep in scope creep: "Stash the extras, finish [core thing], ship it."
- If they're mostly on track with minor drift: "You're close. Drop [specific thing] and you're done."
- If they're on track: "Ship it."
- If new requirements genuinely changed the scope: "The scope changed — that's fine, but name the new scope before continuing. What are you actually building now?"

## Debrief nudge

Only if `debrief-nudges: true` in `.claude/ship.local.md`. Skip this section entirely if the flag is false or absent.

Check `last-debrief`. If it's been more than 7 days (or is `never`), add a single line at the end:

> It's been [N days / a while] since your last debrief. Run `/ship:debrief` when you have 10 minutes.

If `debrief-deferred` is set and it's been more than 2 days since deferral:

> You deferred your debrief [N days] ago. Time to do it — `/ship:debrief`.

## Guidelines

- This is a mirror, not a judge. The tone is "here's what I see" not "you messed up."
- Be concrete. Name files, name changes. Don't say "some of your changes are out of scope" — say which ones.
- If the user has been working for a while and everything is core, acknowledge that. Not every session has scope creep.
- Never suggest adding more scope. The only valid directions are: finish, cut, or redefine.
- Keep the output scannable. The user is mid-flow — they need to read this in 30 seconds and get back to work.
