# Gygabyte Skills

Agent Skills for AI coding agents, by [@gygabyte.lab](https://www.instagram.com/gygabyte.lab) ([razor322](https://github.com/razor322) on GitHub).

[![skills.sh](https://skills.sh/b/razor322/gygabyte-skills)](https://skills.sh/razor322/gygabyte-skills)

| Skill | Use it for |
| --- | --- |
| `ai-assisted-coding` | Build and debug apps with an AI coding agent using Lauren Tan's GrokBot discipline — clean workarounds, no comment crutches, mistakes become lint/CI rules, one standard pattern, one reusable verification CLI, and a Feature Map |
| `core-engineering-standards` | Write, review, and refactor production-grade code — SRP, short functions with early returns, explicit typing, no hardcoded values, 3-layer architecture, explicit error handling, schema validation, and minimal diffs |

## Installation

### With the skills CLI (recommended — works for 40+ agents)

```bash
# all skills in this repo
npx skills add razor322/gygabyte-skills

# one skill only
npx skills add razor322/gygabyte-skills --skill ai-assisted-coding
npx skills add razor322/gygabyte-skills --skill core-engineering-standards

# global, for all projects
npx skills add razor322/gygabyte-skills -g
```

### Claude Code (plugin)

```sh
claude plugin marketplace add razor322/gygabyte-skills
claude plugin install gygabyte@gygabyte-skills
```

Inside a Claude Code session, the equivalent commands are `/plugin marketplace add razor322/gygabyte-skills` and `/plugin install gygabyte@gygabyte-skills`. Start a new session, then use `/skills` to check the installation or `/gygabyte:ai-assisted-coding` to invoke the skill.

### Codex (plugin)

```sh
codex plugin marketplace add razor322/gygabyte-skills
codex plugin install gygabyte@gygabyte-skills
```

Inside a Codex session, the equivalent commands are `/plugin marketplace add razor322/gygabyte-skills` and `/plugin install gygabyte@gygabyte-skills`.

### From npm

```bash
npm install gygabyte-skills
# skill files live in:
# node_modules/gygabyte-skills/skills/ai-assisted-coding/SKILL.md
# node_modules/gygabyte-skills/skills/core-engineering-standards/SKILL.md
```

Atau install pakai skills CLI:

```sh
npx skills add razor322/gygabyte-skills
```

Then copy that folder into your agent's skills directory, for example:

```bash
# Claude Code (project)
cp -r node_modules/gygabyte-skills/skills/. .claude/skills/
# Claude Code (global)
cp -r node_modules/gygabyte-skills/skills/. ~/.claude/skills/
# Codex
cp -r node_modules/gygabyte-skills/skills/. .agents/skills/
# opencode / OpenCode
cp -r node_modules/gygabyte-skills/skills/. .opencode/skills/
```

### Manual installation

Copy each entire leaf skill directory, including its references and assets, into your agent's skill directory. See the npm section above for the paths per agent.

### No agent skills support? Use the project rules file

Copy `templates/AGENTS.md` into your project root as `AGENTS.md` (or `CLAUDE.md`) and fill in the project-specific standards. Rules from both skills, no skill loader needed.

## Skills

### core-engineering-standards

Write, review, and refactor production-grade code in any language or framework. Covers clean idiomatic code, SRP, functions under 30-40 lines, early returns with at most two nesting levels, explicit typing, no hardcoded values, presentation/application/infrastructure layering with no cross-contamination, explicit error handling with structured dev logs and sanitized user messages, schema validation at external boundaries, and surgical minimal diffs. Works alongside `ai-assisted-coding`: that skill governs how the agent works with AI, this one governs how the code is written.

Full instructions: [`skills/core-engineering-standards/SKILL.md`](skills/core-engineering-standards/SKILL.md)

### ai-assisted-coding

Based on Lauren Tan (@poteto), GrokBot engineer at SpaceXAI, via the @christina.atelier TikTok carousel *"How A GrokBot Engineer Utilizes AI for Coding"*. Lauren shipped 2,000 PRs in a month (her X post says 2,500) — the number was not the target, it followed from reusable tools, skills, a codebase AI can understand, and a verification system.

The 6 rules:

1. **Clean the codebase first** — AI copies patterns it sees. Remove temporary workarounds, deprecated and duplicate implementations, dead code, and hacks kept alive by comments.
2. **No comment crutches** — encode constraints in the API/architecture, rules in lint, business rules in tests.
3. **Every repeated AI mistake becomes a rule** — lint rule, compiler check, or CI validation, recorded in the project rules file.
4. **One standard pattern per job** — one way to build an API endpoint, access the DB, handle errors, log, and structure a feature/module.
5. **One reusable verification CLI** — run, control, debug, trace, and prove a change works through the same CLI every session, not a new throwaway script each time.
6. **Keep a Feature Map** — name, function, how to reach it, shortcuts, DOM selectors, interactions, expected result — so AI stops guessing from vague screenshots.

Full instructions: [`skills/ai-assisted-coding/SKILL.md`](skills/ai-assisted-coding/SKILL.md)

## Contents

```
gygabyte-skills/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
├── AGENTS.md
├── README.md
├── LICENSE
├── package.json
├── skills/
│   ├── ai-assisted-coding/
│   │   ├── SKILL.md
│   │   ├── references/feature-map-template.md
│   │   └── assets/AGENTS.md
│   └── core-engineering-standards/
│       ├── SKILL.md
│       └── assets/AGENTS.md
└── templates/
    └── AGENTS.md
```

## skills.sh listing

Skills appear on [skills.sh](https://skills.sh) automatically through anonymous telemetry when someone runs `npx skills add razor322/gygabyte-skills`. No manual submission. Security audits are generated after the first install. Until the first install, the skills.sh page stays empty; the badge above is just a ready link.

## Adding a new skill

1. Create `skills/<name>/SKILL.md` with valid frontmatter (`name` must match the folder name).
2. Add a row to the catalog table above.
3. Bump `version` in `package.json`, `.claude-plugin/plugin.json`, and `.codex-plugin/plugin.json`, plus `metadata.version` in each `SKILL.md`.
4. Publish: `npm publish --access public` (npm) and push to GitHub (for `npx skills add` and plugin marketplaces).

## License

MIT — see [LICENSE](LICENSE).
