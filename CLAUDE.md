# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Ship is a Claude Code plugin (skills-only, no agents/hooks/MCP) that fights scope creep and over-engineering. Five skills: init, think, focus, review, debrief.

- URL: https://ship.withqwerty.com
- Repo: github.com/withqwerty/ship
- Marketplace: github.com/withqwerty/plugins (catalog pointing to this repo + nutmeg)
- Landing page: github.com/withqwerty/ship-site (separate repo, Cloudflare Pages project `ship-www`)
- Also installable via Vercel skills CLI: `npx skills add withqwerty/ship`

## Repository Structure

```
ship/
├── .claude-plugin/
│   ├── plugin.json           # Plugin metadata (name, version, author)
│   └── marketplace.json      # Marketplace listing (name: withqwerty)
├── plugin.json               # Vercel skills CLI manifest
├── skills/
│   ├── init/SKILL.md         # Profile + product brief setup
│   ├── think/SKILL.md        # Pre-build gut check (cascading questions)
│   ├── focus/SKILL.md        # Mid-build scope check via git diff
│   ├── review/SKILL.md       # Pre-ship code review (brutal/kind modes)
│   └── debrief/SKILL.md      # Weekly check-in, updates product brief
├── .gitignore                # Excludes evals/
├── README.md
└── CLAUDE.md
```

No build step. No dependencies. No runtime code. Five markdown files in directories.

## Skills Architecture

Each `SKILL.md` has frontmatter: `name`, `description` (trigger phrases), `argument-hint`, `allowed-tools`. The description field controls when Claude Code activates the skill. Descriptions should be pushy (imperative "Use this skill when...") and include explicit DO NOT trigger boundaries between skills.

**Skill flow:**
- `/ship:init` creates `.claude/ship.local.md` (profile + product brief). All other skills read it.
- `/ship:think` states assumptions, then asks cascading questions (already have it? > what do you mean? > right thing? > right approach?). Can say "just build it."
- `/ship:focus` reads git diff, categorises changes as Core/Adjacent/Drift, tells you what to cut.
- `/ship:review` does one pass, inline (no agent dispatch). Brutal or kind mode. Ends with a verdict.
- `/ship:debrief` reviews git history, asks product-level questions, updates the product brief.

**Profile file (`.claude/ship.local.md`):**
- YAML frontmatter: role, project-stage, default-review-mode, build-priorities, always-flag, debrief-nudges, last-debrief
- Markdown body: product name, one-sentence description, "What moves the needle" (2-3 outcomes), "Current reality" (honest status). Written in Jason Fried style. Evolved by debrief.

**Debrief nudge system (opt-in):** If `debrief-nudges: true`, think/focus/review check `last-debrief` and nudge after 7 days. Max deferral: 2 days.

## Voice and Philosophy

- Direct, opinionated, anti-ceremony. Inspired by DHH/Jason Fried.
- Product-first, not engineering-first. The debrief cares about what moved the needle, not code quality.
- "Just build it" and "Ship it. Clean diff." are valid outputs. Never manufacture objections.
- Never suggest plans, design docs, specs, or more process.
- Findings must be specific (file + line), not vague.

## Contributing

- One skill = one concern. Don't combine.
- Keep skills lean. No `references/` directories.
- No agents, hooks, or MCP servers. Skills only.
- No dependencies or build steps.
- Test locally: `claude --plugin-dir /path/to/ship`

## Related Repos

| Repo | Purpose |
|------|---------|
| withqwerty/ship | This repo. The plugin. |
| withqwerty/ship-site | Landing page (ship.withqwerty.com). Astro static site on Cloudflare Pages project `ship-www`. |
| withqwerty/plugins | Marketplace catalog. Points to ship + nutmeg. |
