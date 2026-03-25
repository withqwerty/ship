# ship

Think. Build. Ship.

A Claude Code plugin that combats your worst instincts as a founder/engineer — the urge to over-scope, over-engineer, and never quite ship. Five skills, zero ceremony.

## Install

```bash
claude --plugin-dir /path/to/ship
```

## Skills

### `/ship:init`

Set up your profile. Four quick questions about who you are and how you like your feedback. Writes `.claude/ship.local.md` — a short file that calibrates everything else. Run it once, edit it anytime.

### `/ship:think`

Before you write a line of code. Reads what you're about to build and asks 3-5 hard questions — the ones you're avoiding. Challenges assumptions, surfaces simpler alternatives, exposes hidden scope. If the idea is good and simple, it'll say so.

### `/ship:focus`

When you're mid-build and drifting. Reads your `git diff`, compares it to what you set out to do, and tells you what's core, what's adjacent, and what's scope creep. Names specific files to cut or stash. Gets you back on track in 30 seconds.

### `/ship:review`

Before you merge. One thorough review covering correctness, security, performance, and simplicity. Two modes:

- **Brutal** (default) — Questions every abstraction. Calls out what shouldn't exist. Assumes you can handle the truth.
- **Kind** — Same eye, different bar. Knows an MVP doesn't need production-grade error handling. Flags what will actually break and lets the rest slide.

Usage: `/ship:review` or `/ship:review kind`

Every review ends with a verdict: **Ship it**, **Fix these first**, or **Needs rework**.

### `/ship:debrief`

Weekly check-in. Reviews your git history, asks a few honest questions, and recalibrates your profile based on how you're actually building — not how you think you're building. Strict, fair, ultimately helpful. Other skills will nudge you when one is due.

## License

MIT
