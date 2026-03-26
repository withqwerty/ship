# ship

Think. Build. Ship.

A Claude Code plugin that combats your worst instincts as a founder/engineer — the urge to over-scope, over-engineer, and never quite ship. Five skills, zero ceremony.

## Install

### Claude Code (via marketplace)

```
/plugin marketplace add withqwerty/plugins
```

Then open `/plugin`, find **ship** in the Discover tab, and install it. Choose **User** scope to make it available across all your projects.

### Any coding agent (via skills CLI)

Works with Claude Code, Cursor, GitHub Copilot, OpenCode, and [40+ other agents](https://github.com/vercel-labs/skills).

```bash
npx skills add withqwerty/ship
```

### Manual

```bash
git clone https://github.com/withqwerty/ship.git
claude --plugin-dir /path/to/ship
```

## Skills

### `/ship:init`

Set up your profile and product brief. A few questions about who you are, what you're building, and what would actually move the needle. Writes `.claude/ship.local.md` — part settings file, part living product brief that every other skill reads. Run it once, evolve it with debriefs.

### `/ship:think`

Before you write a line of code. Reads what you're about to build, checks it against your needle-movers, and asks 3-5 hard questions — the ones you're avoiding. If the idea is good and simple, it'll say so.

### `/ship:focus`

When you're mid-build and drifting. Reads your `git diff`, compares it to what you set out to do, and tells you what's core, what's adjacent, and what's scope creep. Names specific files to cut or stash.

### `/ship:review`

Before you merge. One thorough review covering correctness, security, performance, and simplicity. Two modes:

- **Brutal** (default) — Questions every abstraction. Calls out what shouldn't exist.
- **Kind** — Same eye, different bar. Flags what will actually break and lets the rest slide.

Usage: `/ship:review` or `/ship:review kind`

Every review ends with a verdict: **Ship it**, **Fix these first**, or **Needs rework**.

### `/ship:debrief`

Weekly check-in. Reviews where your time actually went, asks what moved the needle, and updates your product brief. The answer might be more code — or it might be "talk to five users before writing another line." Strict, fair, ultimately helpful.

## Contributing

Pull requests welcome. The bar is high — this plugin exists to fight complexity, so any addition needs to earn its place.

**Before opening a PR:**
- Read the existing skills to understand the voice and structure
- One skill should do one thing. Don't combine concerns.
- Keep skills lean. If it needs a `references/` directory, it's probably too complex.
- Test your changes by installing locally: `claude --plugin-dir /path/to/ship`

**Good contributions:**
- Sharpening existing skill prompts based on real usage
- Fixing edge cases in how skills read the profile or git history
- Improving trigger phrases so skills activate more reliably

**Please don't:**
- Add new skills without discussing first (open an issue)
- Add agents, hooks, or MCP servers — the plugin is intentionally skills-only
- Add dependencies or build steps — it's markdown files in a directory

## License

MIT
