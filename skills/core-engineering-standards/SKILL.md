---
name: core-engineering-standards
description: Write production-grade code with strict engineering standards — single responsibility, functions under 30-40 lines, early returns with at most two nesting levels, explicit typing, no hardcoded values, layered architecture with no cross-contamination, explicit error handling, schema validation at external boundaries, and surgical minimal diffs. Use when writing, reviewing, or refactoring code in any language or framework.
license: MIT
compatibility: Language-agnostic. Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.2.0"
  tags: ["engineering-standards", "clean-code", "architecture", "error-handling", "refactoring", "security"]
---

# Core Engineering Standards

Production-grade, readable, maintainable code. Apply these rules to every file you write or change, in any language or framework. When a rule conflicts with an established codebase pattern, follow the codebase pattern and flag the conflict to the user instead of silently mixing styles.

## 1. Clean and idiomatic code

- Write in the idiomatic style of the target language and framework. Do not invent a personal style on top of its conventions.
- Single Responsibility Principle: each module, class, and function has one reason to change.
- Keep functions under 30-40 lines. Extract a helper when a function grows past that, not when it becomes "hard to read".
- Use early returns and guard clauses. Handle edge cases, invalid inputs, and error states at the top of the function; keep nesting to at most two levels.
- Enforce explicit typing across boundaries. Avoid `any`, `interface{}` without type assertions, and untyped maps/dictionaries unless strictly required — and if required, say why in the reply, not in a comment excusing it.
- Extract constants, status codes, magic strings, and configuration values into dedicated constant files or enums. No hardcoded values.

## 2. Architecture and separation of concerns

- Presentation layer: UI components, pages, and controllers. Render data and translate user input; nothing more.
- Application layer: use cases and orchestration. Own business rules and coordinate infrastructure.
- Infrastructure layer: database queries, API clients, caching, and third-party integrations.
- No cross-contamination: never execute raw database queries, network calls, or heavy business logic inside a UI component or presentation controller.
- Split a file into smaller, focused modules once it approaches 200-250 lines.

## 3. Error handling and resilience

- Never swallow errors. No empty `catch`/`except` blocks, no silent failures.
- Log actionable, structured errors for developers. Return sanitized, user-friendly messages to clients.
- Validate every external input — API payloads, query parameters, file uploads — with a standard schema validator before it reaches domain logic.

## 4. Refactoring guardrails

- Scope discipline: modify or generate only code directly related to the user's prompt. No unsolicited broad refactoring of untouched files.
- Use intention-revealing names for functions, variables, and modules.
- Comments, if any, explain complex business "why", not "how". Never use a comment to excuse a workaround or to restate a rule that belongs in lint or a test.
- Sanitize at trust boundaries: prevent SQL injection, XSS, and command injection. Never hardcode secrets.

## 5. Enforceable architecture

- Encode hard-won lessons as enforceable constraints, not advice. Anything that requires nuance to use well tends to produce slop — in human code and especially in agent-written code.
- **Ban nuance-heavy APIs in product code and provide a safe wrapper instead.** If an API is easy to misuse, forbid its direct use outside the framework/library layer that wraps it safely, and enforce the ban with lint. Example from the Grok Bot team (Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026): React `useEffect` is banned in product code and allowed only inside the Dune framework APIs — the problem is eliminated categorically instead of being caught again in every review.
- **Make the correct structure the obvious one through directory structure.** Name directories so the right place for code is self-evident, then enforce the boundary mechanically. Example: Grok Bot/Dune keeps `electron/main` and `electron/renderer` as two top-level directories, and imports between them are forbidden by a dependency-graph analyser plus lint rules — this stopped agents importing main-thread code into the renderer thread, which had caused long tasks and jank.
- **Enforce layer boundaries mechanically, not by convention alone.** A boundary that only exists in a document is a boundary an agent will cross. Use lint rules, dependency analysis, or the module system to forbid the wrong import/call, with a helpful error message that points at the safe alternative.
- Detail and examples: see `references/enforceable-architecture.md`.

## How to apply

Before writing: identify which layer the change belongs to and name the single responsibility of the new function or module. Check whether a lint rule or dependency boundary already forbids the wrong approach — if not, and the mistake is one an agent could repeat, propose adding one.

While writing: keep functions under 30-40 lines, nesting at most two levels, types explicit at boundaries, and constants extracted instead of inlined.

Before finishing: check file length (split at 200-250 lines), confirm no error is swallowed, confirm every external input is validated, and confirm no secret or hardcoded value slipped in.

When refactoring: stay in scope. If a neighbouring file violates these standards but is untouched by the task, report it instead of refactoring it.

## Relationship to ai-assisted-coding

This skill governs how the code is written. `ai-assisted-coding` governs how the agent works with AI: clean the codebase first, encode rules in lint/tests instead of comments, turn repeated mistakes into rules, keep one standard pattern per job, use one reusable verification CLI, and maintain a Feature Map. Use both on the same task. Where they overlap on comments, both agree — a comment explains complex business "why", never an excuse for a workaround or a rule that belongs in lint or a test.

## Project rules template

If the project has no AGENTS.md / CLAUDE.md yet, copy `assets/AGENTS.md` into the project root and fill in the project-specific standards. That file is the portable version of this skill for agents that do not load skills.
