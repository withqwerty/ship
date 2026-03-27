# ship

Think. Build. Ship.

A plugin for AI coding tools that stops you from over-building and helps you actually ship.

**[ship.withqwerty.com](https://ship.withqwerty.com)**

## Quick start

```
/plugin marketplace add withqwerty/plugins
/plugin install ship@withqwerty
/ship:init
```

Three commands. The init asks you a few questions and writes a small file that all the other commands read. Then just use the commands when you need them — they're not a workflow, they're tools in a drawer.

## What it does

Five commands that intervene at the moments that matter:

- **`/ship:init`** — Tell it who you are, what you're building, and what matters. Takes 2 minutes. It remembers.
- **`/ship:think`** — Before you build something, it asks the hard questions you're avoiding. Sometimes it just says "go build it."
- **`/ship:focus`** — When you're mid-build and the thing keeps growing, it looks at your git diff and tells you what to cut.
- **`/ship:review`** — Before you merge, it reviews your code. Two modes: brutal (high bar) or kind (flags only what'll break).
- **`/ship:debrief`** — Weekly check-in. Looks at what you actually shipped, asks if your priorities still match your behavior, updates your profile.

## Other ways to install

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
