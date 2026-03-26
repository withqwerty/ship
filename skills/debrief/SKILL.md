---
name: ship-debrief
description: This skill should be used when the user wants a weekly check-in on how they're building. Triggers on "ship debrief", "weekly debrief", "how am I doing", "debrief with dhh", "update my profile", "ship retro", or when another ship skill nudges that a debrief is overdue. Reviews recent work, asks a few questions, and updates .claude/ship.local.md.
argument-hint: (no arguments needed)
allowed-tools: Read, Glob, Grep, Bash, AskUserQuestion, Write
---

# Ship Debrief

A periodic check-in that helps you reorient to what matters most. This is not a code review of the week. It's not an engineering retrospective. It's a conversation about where you spent your time, whether it mattered, and what to do next — even if the answer isn't more engineering.

## Process

### 1. Read the profile

Read `.claude/ship.local.md`. If it doesn't exist, tell the user to run `/ship:init` first and stop.

### 2. Review recent work

Ask the user: "Just this repo, or all your repos?" Default to current repo if they say they don't care.

For the chosen scope, review the last 7 days of git history. But don't just catalogue commits — understand the *story* of the week:

- Where did the time actually go? (which areas, which features, which rabbit holes)
- What shipped to users vs. what was internal/preparatory work?
- Was the work connected to the stated priorities in the profile, or did it drift?
- What's the ratio of product work (things users see) to engineering work (things only devs see)?

The git history is evidence, not the point. Use it to understand what the user actually spent their week on.

### 3. Ask the user

After reviewing, ask these questions one at a time:

1. What moved the needle this week? (Not what you're proudest of — what actually mattered for the product/business.)
2. What should you have spent more time on but didn't?
3. Has anything changed about what matters most right now?

Keep it conversational. These questions are about the product and priorities, not the code.

### 4. Synthesise

Combine the git evidence with the user's answers. Think at the product/business level, not the code level. Look for:

- **Priority alignment** — Did the work match the stated priorities? If the profile says "growing daily users" but the week was spent on internal tooling, name that. Not necessarily wrong — but name it.
- **Product vs. plumbing** — How much of the week was user-facing vs. infrastructure? A week of pure plumbing after a month of shipping features is fine. Three weeks of plumbing with no user-facing output is a pattern.
- **What's NOT engineering** — Sometimes the most important thing to do next week isn't code. It might be outreach, user research, content, partnerships, or talking to customers. If the git history suggests the user is hiding in code to avoid harder product/business work, say so.
- **Priority shifts** — The user's answers may reveal that priorities have genuinely changed. Update the profile to match reality.
- **Wins** — What actually moved things forward. Be specific, but frame it in terms of impact, not engineering elegance.

### 5. Deliver the debrief

Strict, fair, ultimately helpful. Not a report — a conversation. Format:

**Where your time went this week:**
[2-3 observations about what the user actually spent the week on. Frame in terms of product impact, not code quality. Name specific work but connect it to whether it matters.]

**What you told me:**
[1-2 sentences connecting their answers to the evidence. Surface gaps between what they say matters and where the time actually went. If their answer about "what moved the needle" doesn't match the git evidence, say so.]

**What matters most right now:**
[Based on everything — is the direction right? Should priorities shift? Is there something non-engineering that needs attention? Be direct. "You should spend two days on outreach before writing another line of code" is a valid suggestion.]

**Suggested profile updates:**
[If priorities, role, project stage, review mode, or always-flag should change, show the specific YAML changes. If nothing needs to change, say so.]

**One thing to focus on next week:**
[A single, concrete suggestion. This might be engineering work, but it might not be. "Run your screenshot tool on every published project — zero code, 10x visual improvement" is better than "refactor the card component." The suggestion should be about what matters most, assuming the current direction of travel is correct.]

### 6. Update the profile

If the user agrees to any suggested changes, update `.claude/ship.local.md` with:
- The agreed profile changes
- `last-debrief: [today's date in YYYY-MM-DD]`
- Remove `debrief-deferred` if it was set

If the user doesn't want to change anything, still update `last-debrief` to today's date.

### 7. Handle deferral

If the user says "not now", "later", "skip", or otherwise defers:
- Set `debrief-deferred: [today's date in YYYY-MM-DD]` in the profile
- Say: "Got it. I'll ask again in a couple of days."
- Do not argue or explain why debriefs are valuable.

The maximum deferral is 2 days. After that, other ship skills will nudge more firmly.

## Guidelines

- This should take 10 minutes, not 30.
- The git history is context, not the deliverable. Don't list commits — tell the story of the week.
- Think product-first, code-second. The user has Claude for code review. They need this for perspective.
- The best suggestion might not be engineering. "Talk to five users" or "write the landing page copy" are valid outputs.
- If someone is shipping the right things, say so. "You're focused on what matters. Keep going." is a valid debrief.
- Never suggest adding more process, tools, or skills. The output is clarity on what matters next.
- Frame everything in terms of impact, not engineering quality. "Clean diffs" is not praise here — "shipped the feature users were asking for" is.
