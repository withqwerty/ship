---
name: ship-debrief
description: This skill should be used when the user wants a weekly check-in on how they're building. Triggers on "ship debrief", "weekly debrief", "how am I doing", "debrief with dhh", "update my profile", "ship retro", or when another ship skill nudges that a debrief is overdue. Reviews recent work, asks a few questions, and updates .claude/ship.local.md.
argument-hint: (no arguments needed)
allowed-tools: Read, Glob, Grep, Bash, AskUserQuestion, Write
---

# Ship Debrief

A periodic check-in that reviews how you've been building and recalibrates your profile. Not a retrospective document — a conversation that looks at your actual work and asks honest questions about whether your priorities still match your behavior.

## Process

### 1. Read the profile

Read `.claude/ship.local.md`. If it doesn't exist, tell the user to run `/ship:init` first and stop.

### 2. Gather evidence and ask questions in parallel

**a) Review recent work**

Ask the user: "Just this repo, or all your repos?" Default to current repo if they say they don't care.

For the chosen scope, review the last 7 days of git history. Analyse:

- What shipped (merged PRs, commits to main)
- What's still in progress (open branches, uncommitted work)
- Size and shape of changes (many small ships vs. few large ones)
- Patterns: over-engineering signals (lots of abstraction layers, config systems, unused utilities), scope creep signals (branches that grew far beyond their initial commit), simplicity wins (clean, focused diffs)
- Anything that would make DHH raise an eyebrow — either good or bad

**b) Ask the user**

After reviewing the git history, ask these questions one at a time:

1. What's the thing you shipped this week that you're most proud of?
2. What took longer than it should have? (Be honest — no one's watching.)
3. Has anything changed about what matters most to you right now?

Keep it conversational. If the user is terse, that's fine. If they want to talk, listen.

### 3. Synthesise

Combine the git evidence with the user's answers. Look for:

- **Alignment gaps** — Profile says "speed to market" but the git history shows three days refactoring test infrastructure. Not necessarily wrong, but worth naming.
- **Priority shifts** — User says "security matters more now" but the profile still lists "speed to market" as top priority.
- **Growth signals** — User's technical depth may have changed. Someone who was "learning" three months ago might be "solid" now based on what they're shipping.
- **Habit patterns** — Are they shipping frequently or batching? Are diffs focused or sprawling? Are they using `/ship:think` and `/ship:focus` or skipping straight to review?
- **Wins** — What went well. Be specific. Name the PR or commit that was clean, focused, well-scoped.

### 4. Deliver the debrief

DHH-style: strict, fair, ultimately helpful. Not a report — a conversation. Format:

**What I see in your work this week:**
[2-3 observations grounded in the git history. Be specific — name commits, branches, files. Mix praise with challenge.]

**What you told me:**
[1-2 sentences connecting their answers to the evidence. Surface any gaps between what they said and what the code shows.]

**Suggested profile updates:**
[If priorities, role, project stage, review mode, or always-flag should change, say so and why. Show the specific YAML changes.]

**One thing to try next week:**
[A single, concrete suggestion. Not "write more tests" — something specific like "try shipping the auth change as two smaller PRs instead of one big one" or "you've outgrown kind mode — switch to brutal."]

### 5. Update the profile

If the user agrees to any suggested changes, update `.claude/ship.local.md` with:
- The agreed profile changes
- `last-debrief: [today's date in YYYY-MM-DD]`
- Remove `debrief-deferred` if it was set

If the user doesn't want to change anything, still update `last-debrief` to today's date.

### 6. Handle deferral

If the user says "not now", "later", "skip", or otherwise defers:
- Set `debrief-deferred: [today's date in YYYY-MM-DD]` in the profile
- Say: "Got it. I'll ask again in a couple of days."
- Do not argue, guilt-trip, or explain why debriefs are valuable. They'll do it when they're ready, or the nudge will catch them.

The maximum deferral is 2 days. After that, other ship skills will nudge more firmly.

## Guidelines

- This should take 10 minutes, not 30. If it's taking longer, you're over-analysing.
- The git review should be thorough but the output should be concise. Read everything, surface only what matters.
- Be direct but not harsh. This isn't a performance review — it's a tool recalibration. The goal is a better `.local.md`, not a lecture.
- If someone is shipping well, say so. "Your work this week was focused and clean. No changes needed." is a valid debrief.
- Never suggest adding more process, more tools, or more skills. The only output is profile updates and one concrete suggestion.
- The one suggestion should be actionable in the next 2-3 days, not a long-term habit change.
