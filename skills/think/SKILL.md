---
name: ship-think
description: This skill should be used when the user is about to start building something and needs a gut check first. Triggers on "ship think", "before I build this", "should I build this", "sanity check this idea", "gut check", or when the user describes a feature they're about to implement and would benefit from hard questions before coding.
argument-hint: <what you're about to build (or leave blank to use conversation context)>
allowed-tools: Read, Glob, Grep, AskUserQuestion
---

# Ship Think

Combat the founder/engineer brain that wants to jump straight to code. This is not a planning phase — it's a short, sharp gut check that surfaces the questions you're avoiding.

## Process

### 1. Understand what they want to build

If the user provided an argument, use that. Otherwise, read the recent conversation context to understand what they're about to work on. If neither is clear, ask one question: "What are you about to build?"

### 2. Read the profile

Check `.claude/ship.local.md` for user context. If it doesn't exist, proceed without it — but mention they can run `/ship:init` to calibrate things.

The profile shapes the questions. Read both the YAML settings and the product brief (the markdown body). The "What moves the needle" section tells you what actually matters for this product right now. If the thing they want to build doesn't connect to those needle-movers, that's your most important question.

### 3. Read the codebase (briefly)

Glance at the project structure and any files directly relevant to what they're describing. The goal is to ground the questions in reality — not to do a full audit. Spend no more than a few file reads here.

### 4. Ask the hard questions

Output 3-5 questions. Not a checklist — a conversation. Each question should:

- Challenge an assumption they're making
- Surface a simpler alternative they might be ignoring
- Force a decision they're deferring
- Expose scope that's hiding inside what sounds simple

**Calibrate to the user.** The profile tells you who they are — match the level of the questions to the level of the person. A non-technical founder needs different scrutiny than a senior engineer. An MVP-stage project gets different questions than a mature codebase. Don't ask a founder about architectural patterns; don't ask a senior engineer if they've considered a no-code tool. Read the room.

**Question format:**

Each question should be 1-2 sentences. Direct. No preamble, no "have you considered...", no softening. Frame them as the questions a brutally honest co-founder would ask.

Example output:

> **1. What's the actual user problem?** You described the feature but not why someone needs it. If you can't say "users are doing X and it's painful because Y", you're building for yourself, not them.
>
> **2. The settings page is scope creep.** You said "simple config" but that's a UI, a persistence layer, and validation. Can this be a config file for now?
>
> **3. You already have something close.** `src/utils/notify.ts` does 60% of what you described. Have you looked at extending it instead of building from scratch?

### 5. Wait

Do not proceed to building. Do not offer to help implement. The output is questions — the user decides what to do with them. If they want to discuss an answer, engage. If they want to start building, they will.

## Debrief nudge

Only if `debrief-nudges: true` in `.claude/ship.local.md`. Skip this section entirely if the flag is false or absent.

Check the `last-debrief` field. If it's been more than 7 days (or is `never`), add a single line at the end:

> It's been [N days / a while] since your last debrief. Run `/ship:debrief` when you have 10 minutes.

If `debrief-deferred` is set and it's been more than 2 days since deferral:

> You deferred your debrief [N days] ago. Time to do it — `/ship:debrief`.

## Guidelines

- This is a conversation, not a document. No headers like "Scope Assessment" or "Risk Analysis."
- Maximum 5 questions. Fewer is better if they're sharp enough.
- Ground at least one question in the actual codebase — show you looked.
- If the idea is genuinely good and simple, say so. Don't manufacture objections. "This is straightforward — just build it" is a valid output.
- Never suggest they need a plan, a design doc, or a spec. That's the ceremony this plugin exists to kill.
