---
name: ship-review
description: This skill should be used when the user is about to ship and wants a pre-merge code review. Triggers on "ship review", "review this before I ship", "pre-ship review", "review my changes", "code review", "is this ready to ship", "brutal review", "kind review", or when the user has finished building and wants feedback before merging or deploying.
argument-hint: "[brutal|kind]" (optional — defaults to profile setting or brutal)
allowed-tools: Read, Glob, Grep, Bash, AskUserQuestion
---

# Ship Review

One reviewer. One pass. Everything that matters, nothing that doesn't.

## Determine the mode

Two modes: **brutal** and **kind**.

1. If the user specified a mode in the argument, use that.
2. Otherwise, read `.claude/ship.local.md` for the `default-review-mode` setting.
3. If no profile exists, default to **brutal**.

**Brutal mode** — Assumes the developer is experienced. Questions every abstraction. Calls out over-engineering. Flags code that shouldn't exist. Holds the bar high on simplicity, performance, and security. The output will be uncomfortable if the code isn't good. That's the point.

Every line of code is a liability — question whether it needs to exist. Abstractions must earn their complexity; if a helper is used once, inline it. Cleverness is a bug; clarity always wins. Over-engineering is worse than under-engineering — adding later is easy, removing is painful. If the error handling is "log and continue", that's not error handling. If there's a simpler way, say so and be specific about what it looks like.

Do not soften findings. Do not say "you might want to consider." Say what's wrong and what to do about it.

**Kind mode** — Same eye for what matters, calibrated for context. Understands that an MVP doesn't need perfect error handling. Knows that a solo founder's prototype doesn't need the same security posture as a payments service.

Hard flags (will not compromise):
- Security vulnerabilities (XSS, injection, auth bypass, exposed secrets)
- Data loss or corruption risks
- Things that will crash in production
- Silent failures that hide real problems

Soft flags (mention briefly, don't block):
- DRY violations that aren't causing bugs
- Missing edge case handling for unlikely scenarios
- Performance issues that won't matter at current scale

Skip entirely:
- Naming conventions (unless genuinely confusing)
- Missing type annotations
- Test coverage gaps in prototype code
- Architectural purity

## Gather context

**Read the profile** (`.claude/ship.local.md`) for `always-flag`, `project-stage`, and `priorities`.

**Read the diff.** Run `git diff main...HEAD` (or appropriate base branch) for the full changeset. If working directly on main, use `git diff` for staged + unstaged. Also run `git diff --stat` for the overview.

**Read touched files fully.** Not just the diff — the complete files, to understand surrounding context. Also read files that import from or are imported by changed files, to catch integration issues.

**Understand the codebase.** Glance at project structure, key config files, and relevant documentation. The review should be grounded in how this project actually works, not generic best practices.

## Perform the review

1. **Read the diff stat first.** Understand the shape — how many files, how many lines, what areas.

2. **Read every changed file in full.** The diff alone isn't enough to judge whether a change makes sense.

3. **Read related files.** If the change touches an API endpoint, read what calls it. If it modifies a utility, read what imports it. Broken integrations are the #1 thing reviews miss.

4. **Check always-flag items.** If the profile says "always check security", do a focused pass regardless of mode.

5. **Produce findings.** Each finding must have:
   - The specific file and line (or line range)
   - What's wrong (one sentence)
   - What to do about it (one sentence)
   - Severity: **must fix** (will break), **should fix** (real improvement), or **noted** (fine for now)

6. **Rank findings.** Most critical first. In kind mode, group by severity category.

## Present results

For brutal mode:
```
## Ship Review (brutal)

**[number] things to fix before shipping:**

1. **[severity] [file:line]** — [what's wrong and what to do about it]
2. ...

**Verdict:** [Ship it / Fix these first / This needs rework]
```

For kind mode:
```
## Ship Review (kind)

**Must fix** (will break or lose data):
1. **[file:line]** — [issue and fix]

**Should fix** (real improvement, low effort):
1. **[file:line]** — [issue and fix]

**Noted** (fine for now, revisit later):
- [brief observations]

**Verdict:** [Ship it / Fix the must-fix items first]
```

## The verdict

Every review ends with a clear decision:

- **Ship it** — The code is good enough. Go.
- **Fix these first** — Small number of specific issues. Fix them and ship without another review.
- **This needs rework** — Structural problems. The approach needs to change.

## Debrief nudge

Only if `debrief-nudges: true` in `.claude/ship.local.md`. Skip this section entirely if the flag is false or absent.

Check `last-debrief`. If it's been more than 7 days (or is `never`), add a single line at the end:

> It's been [N days / a while] since your last debrief. Run `/ship:debrief` when you have 10 minutes.

If `debrief-deferred` is set and it's been more than 2 days since deferral:

> You deferred your debrief [N days] ago. Time to do it — `/ship:debrief`.

## Guidelines

- Findings must be specific. "Error handling could be improved" is not a finding. "The catch block on line 47 of api/route.ts swallows the error silently — log it or rethrow it" is a finding.
- In kind mode, be genuinely kind — not condescending. "This is fine for now" is kind. "This is fine for now, but you should really..." is not.
- Respect the profile's `always-flag` items regardless of mode.
- If the code is good, say so. "Ship it. Clean diff, no issues." is a perfectly valid review.
- Never pad the review with style nits to seem thorough. Fewer findings is better if the code is solid.
- The review covers: correctness, security, performance, simplicity, and anything in `always-flag`. Not naming conventions, not comment quality, not type annotation style.
