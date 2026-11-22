# <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/layers.svg" width="24" height="24" alt="Layers"> Gygabyte Skills

Agent Skills for AI coding agents, by [@gygabyte.lab](https://www.instagram.com/gygabyte.lab) ([razor322](https://github.com/razor322) on GitHub).

[![npm version](https://img.shields.io/npm/v/gygabyte-skills?color=cb3837&logo=npm)](https://www.npmjs.com/package/gygabyte-skills)
[![license](https://img.shields.io/npm/l/gygabyte-skills)](https://github.com/razor322/gygabyte-skills/blob/main/LICENSE)
[![skills.sh](https://skills.sh/b/razor322/gygabyte-skills)](https://skills.sh/razor322/gygabyte-skills)

**[English](README.md)** · [Bahasa Indonesia](README.id.md)

| Skill | Use it for |
| --- | --- |
| `ai-assisted-coding` | Build and debug apps with an AI coding agent using Lauren Tan's GrokBot discipline — clean workarounds, no comment crutches, mistakes become lint/CI rules, one standard pattern, one reusable verification CLI, a Feature Map, the trust pyramid, and verification-first token spend |
| `core-engineering-standards` | Write, review, and refactor production-grade code — SRP, short functions with early returns, explicit typing, no hardcoded values, 3-layer architecture, explicit error handling, schema validation, and minimal diffs |
| `enforceable-architecture` | Turn hard-won lessons into enforceable architecture constraints instead of advice — ban nuance-heavy APIs in product code with a safe wrapper, make the correct structure obvious through directory layout, enforce layer boundaries mechanically |
| `runtime-verification` | Give the agent a way to run the app itself and prove a change works — one reusable CLI/driver, a Feature Map as shared vocabulary, and video, screenshot, and state-chart evidence instead of reviewing diffs alone |
| `formal-verification` | Add a formal-verification layer on top of tests — model-check the most important stateful flows and prove key algorithms — as one more layer alongside runtime verification, not a silver bullet |
| `software-factory` | Build the outer automation loop only after the inner coding loop is solid — release-triggered fuzzing, auto-fix PRs, and automerge — because automating a slop-producing loop just produces more slop |

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/trending-up.svg" width="18" height="18" alt="Trending up"> What changes when you use these skills?

Picture two developers given the exact same task. One uses these skills, one does not. Both versions **work** — but the one built with the skills is cleaner, safer, and far easier for the next person (or the next AI) to continue.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/search.svg" width="18" height="18" alt="Search"> The difference you notice right away

- **Code stops nesting like a pyramid** — deeply stacked `if`s become a flat, readable flow.
- **Every function is explicitly typed** — parameters and return values are declared.
- **No "magic" numbers or strings** — each one gets a name in a single place, easy to change.
- **No "do not touch" comments or `TODO`s** — rules live in lint and tests, not in sticky notes.
- **There is a way to prove it works** — one verification command, plus a feature map so the AI stops guessing.

> These are illustrative results from small tasks we tried (user registration, an API endpoint, and one file from a real project). They are not a guaranteed score for your codebase — outcomes still depend on your project and how you use the AI. What is shown here is the **shape of the impact**, not a promised number.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/package.svg" width="18" height="18" alt="Package"> Installation

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/wrench.svg" width="18" height="18" alt="Wrench"> With the skills CLI (recommended — works for 40+ agents)

```bash
# all skills in this repo
npx skills add razor322/gygabyte-skills

# one skill only
npx skills add razor322/gygabyte-skills --skill ai-assisted-coding
npx skills add razor322/gygabyte-skills --skill core-engineering-standards
npx skills add razor322/gygabyte-skills --skill runtime-verification

# global, for all projects
npx skills add razor322/gygabyte-skills -g
```

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt="Puzzle"> Claude Code (plugin)

```sh
claude plugin marketplace add razor322/gygabyte-skills
claude plugin install gygabyte@gygabyte-skills
```

Inside a Claude Code session, the equivalent commands are `/plugin marketplace add razor322/gygabyte-skills` and `/plugin install gygabyte@gygabyte-skills`. Start a new session, then use `/skills` to check the installation or `/gygabyte:ai-assisted-coding` to invoke the skill.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt="Puzzle"> Codex (plugin)

```sh
codex plugin marketplace add razor322/gygabyte-skills
codex plugin install gygabyte@gygabyte-skills
```

Inside a Codex session, the equivalent commands are `/plugin marketplace add razor322/gygabyte-skills` and `/plugin install gygabyte@gygabyte-skills`.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/package.svg" width="18" height="18" alt="Package"> From npm

```bash
npm install gygabyte-skills
# skill files live in:
# node_modules/gygabyte-skills/skills/ai-assisted-coding/SKILL.md
# node_modules/gygabyte-skills/skills/core-engineering-standards/SKILL.md
```

Or install with the skills CLI:

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

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/hammer.svg" width="18" height="18" alt="Hammer"> Manual installation

Copy each entire leaf skill directory, including its references and assets, into your agent's skill directory. See the npm section above for the paths per agent.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/scroll-text.svg" width="18" height="18" alt="Scroll text"> No agent skills support? Use the project rules file

Copy `templates/AGENTS.md` into your project root as `AGENTS.md` (or `CLAUDE.md`) and fill in the project-specific standards. Rules from all six skills, no skill loader needed.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt="Puzzle"> Skills

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/code.svg" width="18" height="18" alt="Code"> core-engineering-standards

Write, review, and refactor production-grade code in any language or framework. Covers clean idiomatic code, SRP, functions under 30-40 lines, early returns with at most two nesting levels, explicit typing, no hardcoded values, presentation/application/infrastructure layering with no cross-contamination, explicit error handling with structured dev logs and sanitized user messages, schema validation at external boundaries, and surgical minimal diffs. Works alongside `ai-assisted-coding`: that skill governs how the agent works with AI, this one governs how the code is written.

Full instructions: [`skills/core-engineering-standards/SKILL.md`](skills/core-engineering-standards/SKILL.md), plus the enforceable-architecture detail in [`skills/core-engineering-standards/references/enforceable-architecture.md`](skills/core-engineering-standards/references/enforceable-architecture.md) and the pre-submit checklist in [`skills/core-engineering-standards/references/checklist.md`](skills/core-engineering-standards/references/checklist.md).

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/bot.svg" width="18" height="18" alt="Bot"> ai-assisted-coding

Based on Lauren Tan (@poteto), GrokBot engineer at SpaceXAI, via the @christina.atelier TikTok carousel *"How A GrokBot Engineer Utilizes AI for Coding"* and her talk *"All Code Is Slop (Except Mine)"* (14 Nov 2026). Lauren shipped 2,000 PRs in a month (her X post says 2,500; in the talk she says 3,000 personally and 11,000 as a team, at a ~0.5% revert rate) — the number was not the target, it followed from reusable tools, skills, a codebase AI can understand, and a verification system.

The rules:

1. **Clean the codebase first** — AI copies patterns it sees. Remove temporary workarounds, deprecated and duplicate implementations, dead code, and hacks kept alive by comments.
2. **No comment crutches** — encode constraints in the API/architecture, rules in lint, business rules in tests.
3. **Every repeated AI mistake becomes a rule** — lint rule, compiler check, or CI validation, recorded in the project rules file.
4. **One standard pattern per job** — one way to build an API endpoint, access the DB, handle errors, log, and structure a feature/module.
5. **One reusable verification CLI** — run, control, debug, trace, and prove a change works through the same CLI every session, not a new throwaway script each time.
6. **Keep a Feature Map** — name, function, how to reach it, shortcuts, DOM selectors, interactions, expected result — so AI stops guessing from vague screenshots.
7. **Trust pyramid** — spend the most effort where it is deterministic: codebase, then static analysis, then verification, then prompts on top. When correcting the agent, turn the correction into a deterministic check first, not another line in the rules file.
8. **Spend most tokens on verification** — verification is the main token spend, not an afterthought; judge a system by revert rate and user-visible performance, not by PR count. Review artefacts (video, screenshots, state charts), not just diffs.

Full instructions: [`skills/ai-assisted-coding/SKILL.md`](skills/ai-assisted-coding/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/shield-check.svg" width="18" height="18" alt="Shield check"> enforceable-architecture

Turn hard-won lessons into enforceable architecture constraints instead of advice. Ban nuance-heavy APIs in product code and provide a safe wrapper (the raw API lives only inside the framework layer — e.g. React `useEffect` in Dune for Grok Bot), make the correct structure obvious through directory layout (e.g. `electron/main` vs `electron/renderer`), and enforce layer boundaries mechanically with lint and dependency analysis. Use when a pattern keeps being misused, an agent keeps crossing a layer boundary, or a rule keeps being ignored.

Full instructions: [`skills/enforceable-architecture/SKILL.md`](skills/enforceable-architecture/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/activity.svg" width="18" height="18" alt="Activity"> runtime-verification

Give the AI agent a way to run the app itself and prove a change works. One reusable CLI/driver instead of a fresh script per session, a Feature Map as shared vocabulary from the user's point of view, and video, screenshot, and state-chart evidence instead of reviewing diffs alone. Most tokens go to verification. Use when verifying a feature, fixing a UI bug, or reviewing agent work.

Full instructions: [`skills/runtime-verification/SKILL.md`](skills/runtime-verification/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/badge-check.svg" width="18" height="18" alt="Badge check"> formal-verification

Add a formal-verification layer on top of tests: model-check the most important stateful flows with TLA+ (e.g. notifications across devices/platforms), prove key algorithms with Lean (e.g. a custom virtualisation library, so it does not flicker or jitter), and catch stateful UI edge cases with property-based testing. One more layer alongside runtime verification — not a silver bullet, and worth it first where a bug is most expensive.

Full instructions: [`skills/formal-verification/SKILL.md`](skills/formal-verification/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/factory.svg" width="18" height="18" alt="Factory"> software-factory

Build the outer automation loop only after the inner coding loop is solid. Order: correct via the codebase first, lint rule first, then clean up legacy patterns, make verification strong — only then automate. The outer loop (e.g. release-triggered fuzzing that uses the app like a real user, opens a fix PR per bug, and automerges) works only when PRs are good by default and the revert rate is low and stable. Automating a slop-producing loop just produces more slop and burns tokens.

Full instructions: [`skills/software-factory/SKILL.md`](skills/software-factory/SKILL.md)

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/folder-tree.svg" width="18" height="18" alt="Folder tree"> Contents

```
gygabyte-skills/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
├── AGENTS.md
├── README.md
├── README.id.md
├── LICENSE
├── package.json
├── skills/
│   ├── ai-assisted-coding/
│   │   ├── SKILL.md
│   │   ├── references/feature-map-template.md
│   │   ├── references/trust-pyramid.md
│   │   └── assets/AGENTS.md
│   ├── core-engineering-standards/
│   │   ├── SKILL.md
│   │   ├── references/checklist.md
│   │   ├── references/enforceable-architecture.md
│   │   └── assets/AGENTS.md
│   ├── enforceable-architecture/
│   │   ├── SKILL.md
│   │   ├── references/examples.md
│   │   └── assets/AGENTS.md
│   ├── runtime-verification/
│   │   ├── SKILL.md
│   │   ├── references/evidence-checklist.md
│   │   └── assets/AGENTS.md
│   ├── formal-verification/
│   │   ├── SKILL.md
│   │   ├── references/when-to-use.md
│   │   └── assets/AGENTS.md
│   └── software-factory/
│       ├── SKILL.md
│       ├── references/factory-checklist.md
│       └── assets/AGENTS.md
└── templates/
    └── AGENTS.md
```

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/globe.svg" width="18" height="18" alt="Globe"> skills.sh listing

Skills appear on [skills.sh](https://skills.sh) automatically through anonymous telemetry when someone runs `npx skills add razor322/gygabyte-skills`. No manual submission. Security audits are generated after the first install. Until the first install, the skills.sh page stays empty; the badge above is just a ready link.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/plus-circle.svg" width="18" height="18" alt="Plus circle"> Adding a new skill

1. Create `skills/<name>/SKILL.md` with valid frontmatter (`name` must match the folder name).
2. Add a row to the catalog table above.
3. Bump `version` in `package.json`, `.claude-plugin/plugin.json`, and `.codex-plugin/plugin.json`, plus `metadata.version` in each `SKILL.md`.
4. Publish: `npm publish --access public` (npm) and push to GitHub (for `npx skills add` and plugin marketplaces).

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/scale.svg" width="18" height="18" alt="Scale"> License

MIT — see [LICENSE](LICENSE).
