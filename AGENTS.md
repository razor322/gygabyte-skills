# Working on Gygabyte Skills

Source of truth for Gygabyte Agent Skills. This repo contains instructions, not an application. `skills/` holds independently installable skills; each leaf directory is one skill.

- Keep every `skills/<name>/SKILL.md` compliant with the [Agent Skills specification](https://agentskills.io/specification): valid YAML frontmatter and `name` matching the immediate directory name.
- Keep skill entrypoints concise. Put detail in `references/` and link from the entrypoint.
- Keep `assets/` self-contained — a skill installed via `npx skills add` only gets its own folder.
- Rule text lives in four places per skill: its `SKILL.md`, its `assets/AGENTS.md`, the combined `templates/AGENTS.md`, and README. When rules change, update all four together.
- Keep links, plugin names, versions, and repo URLs consistent across `package.json`, `.claude-plugin/`, `.codex-plugin/`, `.agents/plugins/`, and README.
- Do not add application scaffolding or dependencies unless needed to validate a skill.
- Before finishing, validate: JSON parses, frontmatter `name` = folder name, relative links resolve, `npm pack --dry-run` includes every file, and no stale repo paths remain. Report what was actually checked.
