---
name: ai-assisted-coding
description: Build and debug apps with an AI coding agent using Lauren Tan's GrokBot discipline — clean workarounds AI would copy, encode rules in lint/tests instead of comments, turn repeated AI mistakes into lint/CI rules, use one standard pattern, verify with one reusable CLI, and keep a Feature Map. Use when building a feature, fixing a bug, refactoring, or setting up AI-assisted development in a project.
license: MIT
compatibility: Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.1.0"
  source: Lauren Tan (@poteto), GrokBot engineer at SpaceXAI, via @christina.atelier on TikTok ("How A GrokBot Engineer Utilizes AI for Coding")
  tags: ["ai-coding", "coding-agent", "refactoring", "lint", "testing", "code-quality"]
---

# AI-Assisted Coding

Source: Lauren Tan (@poteto), GrokBot engineer at SpaceXAI. Lauren shipped 2,000 PRs in a month (her X post says 2,500) — the number was not the target, it followed from reusable tools, skills, a codebase AI can understand, and a verification system.

Use this skill whenever the user asks to build, change, or debug an app with an AI agent. Apply it before writing new code, not as a cleanup step afterwards.

## 1. Clean the codebase before letting AI copy it

- AI imitates patterns already in the codebase, so a messy codebase teaches bad patterns.
- Before a coding task, remove or flag:
  - Temporary workarounds — AI can treat them as the standard solution.
  - Deprecated implementations — AI may reuse them.
  - Duplicate implementations — AI cannot tell which one is standard.
  - Dead code / obsolete flows — they add noise and irrelevant examples.
  - Hacks kept alive by a comment — they teach AI to keep the temporary fix.
- If a workaround cannot be removed yet, say so explicitly in the task and name the correct pattern to use instead. Do not leave the workaround as the only example.

## 2. Do not use comments as a crutch

- Do not write comments to explain a constraint, rule, or business rule and expect AI or a developer to remember it. AI can use a comment as justification for not fixing the root cause.
- Encode intent in a place that enforces it:
  - Constraint → put it in the API or architecture, so the wrong use is impossible or obvious.
  - Rule → enforce it through lint.
  - Business rule → prove it through a test.
- A comment is acceptable only for genuinely non-obvious "why" that cannot be expressed in code, lint, or a test. Never use a comment to excuse a workaround.

## 3. Turn every repeated AI mistake into a rule

- When AI makes the same mistake twice, do not just correct that instance.
- After fixing it, add a durable guard in this order of preference:
  1. Lint rule
  2. Compiler / type check
  3. CI validation
- Record the new rule in the project's rules file (AGENTS.md / CLAUDE.md) so the next session inherits it.
- Tell the user what rule was added and what mistake it prevents.

## 4. Keep exactly one standard pattern per job

- Do not let AI choose between several implementation patterns. Provide one recommended pattern for each of:
  - API endpoint
  - Database access
  - Error handling
  - Logging
  - Feature / module structure
- If the codebase already has more than one pattern, name the standard one in the task and do not present the others as options.
- When a new pattern is genuinely needed, add it as the new single standard and migrate or explicitly deprecate the old one — do not leave both live.

## 5. Verify with one reusable CLI, not a new script per session

- Do not let the agent write a fresh test / run / debug script every session. Results and verification then vary between sessions.
- Build or reuse one CLI the agent uses to:
  - Run the app
  - Control the app
  - Debug it
  - Take a performance trace
  - Collect proof that a change actually works
- Every change must be verified through that CLI, in the same way, before it is presented as done.
- If no such CLI exists for the project, build that first (or propose it as the first task) instead of writing ad-hoc scripts per feature.

## 6. Keep a Feature Map so AI does not guess

- Maintain a Feature Map for the app, covering each feature:
  - Name and function
  - How the user reaches it
  - Keyboard shortcuts
  - DOM elements / selectors used
  - Interactions to perform
  - Expected result
- Use the Feature Map when investigating a bug, especially when the report is a vague screenshot. Point the agent to the exact feature entry instead of letting it guess from the UI.
- Update the Feature Map when a feature is added, renamed, or moved. A stale map is worse than no map if it points at the wrong selector.
- Template: see `references/feature-map-template.md`.

## Workflow for a new feature or fix

1. Audit: check for workarounds, duplicates, and dead code in the touched area first.
2. Standard: name the single pattern to use for endpoint, DB, error handling, logging, and module structure.
3. Build: write code with no excuse-comments; encode rules in API, lint, or tests.
4. Verify: run the change through the reusable CLI and collect proof it works.
5. Harden: if AI made a repeated mistake, add a lint / compiler / CI rule before closing the task.
6. Update: update the Feature Map if the user-facing behaviour changed.

## Project rules template

If the project has no AGENTS.md / CLAUDE.md yet, copy `assets/AGENTS.md` into the project root and fill in the project-specific standards (the one API pattern, the one DB pattern, the verify command). That file is the portable version of this skill for agents that do not load skills.
