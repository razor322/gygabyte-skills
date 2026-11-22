# AI-Assisted Coding Rules

Rules for any agent working in this project. Fill in the placeholders with this project's actual standards.

## 0. Trust pyramid

Spend time in this order: (1) the codebase itself — API design, architecture, refactoring; (2) static analysis — lint, types, compiler, tests; (3) verification — runtime and formal; (4) prompts/rules/skills — the smallest layer. When correcting the agent, first turn the correction into a deterministic check (lint, type check, CI), not another line in this file.

## 1. Clean before copying

AI imitates what it sees. Before changing code, remove or flag temporary workarounds, deprecated implementations, duplicates, dead code, and hacks kept alive by comments. If a workaround must stay, name the correct pattern to use instead.

## 2. No comment crutches

- Constraint → encode it in the API or architecture, so the wrong use is impossible or obvious.
- Rule → enforce it with lint.
- Business rule → prove it with a test.

Comments are acceptable only for genuinely non-obvious "why" that code, lint, or a test cannot express. Never use a comment to excuse a workaround.

## 3. Repeat → rule

Same AI mistake twice → add a durable guard, in this order of preference: lint rule, compiler/type check, CI validation. Record the new rule here and tell the user what mistake it prevents.

## 4. One pattern per job

Use exactly one standard pattern for each job:

- API endpoint: `<fill in>`
- Database access: `<fill in>`
- Error handling: `<fill in>`
- Logging: `<fill in>`
- Feature / module structure: `<fill in>`

If the codebase already has more than one pattern, name the standard one and do not present the others as options. A new pattern replaces the old one; it does not sit beside it.

## 5. One verification CLI

Command: `<fill in>`

Every change must be verified through this CLI before it is called done. Use it to run, control, debug, trace, and collect proof. Do not write a throwaway script per session. If no such CLI exists, build it first.

Spend most tokens on verification, not on writing code. Collect proof as review artefacts — video, screenshots, diagrams, or a state chart of the behaviour from the user's point of view — not just a passing test run. Judge the system by revert rate and user-visible performance, not by PR count.

## 6. Feature Map

Map location: `<fill in>`

Keep it current: name, function, how to reach it, shortcuts, DOM selectors, interactions, expected result. Point the agent at an entry instead of letting it guess from a vague screenshot.

## Workflow

1. Audit the touched area for workarounds, duplicates, and dead code.
2. Name the single pattern for endpoint, DB, error handling, logging, and module structure.
3. Build with no excuse-comments; encode rules in the API, lint, or tests.
4. Verify through the CLI and collect proof — video, screenshots, or a state chart.
5. Add a lint/compiler/CI guard for any repeated mistake.
6. Update the Feature Map when user-facing behaviour changes.
