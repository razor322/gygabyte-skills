# Project Rules — AI-Assisted Coding + Core Engineering Standards

Rules for any agent working in this project. Fill in the placeholders with this project's actual standards. Copy this file to the project root as `AGENTS.md` or `CLAUDE.md`.

## Part 1 — AI-assisted coding

### Clean before copying

AI imitates what it sees. Before changing code, remove or flag temporary workarounds, deprecated implementations, duplicates, dead code, and hacks kept alive by comments. If a workaround must stay, name the correct pattern to use instead.

### No comment crutches

- Constraint → encode it in the API or architecture, so the wrong use is impossible or obvious.
- Rule → enforce it with lint.
- Business rule → prove it with a test.

Comments are acceptable only for genuinely non-obvious "why" that code, lint, or a test cannot express. Never use a comment to excuse a workaround.

### Repeat → rule

Same AI mistake twice → add a durable guard, in this order of preference: lint rule, compiler/type check, CI validation. Record the new rule here and tell the user what mistake it prevents.

### One pattern per job

Use exactly one standard pattern for each job:

- API endpoint: `<fill in>`
- Database access: `<fill in>`
- Error handling: `<fill in>`
- Logging: `<fill in>`
- Feature / module structure: `<fill in>`

If the codebase already has more than one pattern, name the standard one and do not present the others as options. A new pattern replaces the old one; it does not sit beside it.

### One verification CLI

Command: `<fill in>`

Every change must be verified through this CLI before it is called done. Use it to run, control, debug, trace, and collect proof. Do not write a throwaway script per session. If no such CLI exists, build it first.

### Feature Map

Map location: `<fill in>`

Keep it current: name, function, how to reach it, shortcuts, DOM selectors, interactions, expected result. Point the agent at an entry instead of letting it guess from a vague screenshot.

## Part 2 — Core engineering standards

### Clean and idiomatic code

- Follow the idiomatic style of the language and framework. No personal style on top of conventions.
- One reason to change per module, class, and function.
- Functions under 30-40 lines. Guard clauses and early returns; nesting at most 2 levels.
- Explicit types at every boundary. Avoid `any`, `interface{}` without type assertions, and untyped maps/dictionaries unless strictly required. State the reason in the reply, not in an excuse-comment.
- Constants, status codes, magic strings, and config values live in dedicated constant files or enums. No hardcoded values.

### Architecture

- Presentation: UI, pages, controllers — render data, translate input.
- Application: use cases, orchestration, business rules.
- Infrastructure: database, API clients, caching, third-party integrations.
- No raw DB query, network call, or heavy business logic in UI components / controllers.
- Split files approaching 200-250 lines into focused modules.

### Error handling

- No empty catch/except, no swallowed errors.
- Structured error logs for developers; sanitized, user-friendly messages for clients.
- Validate all external inputs (payloads, query params, uploads) with a schema validator before domain logic.

### Refactoring guardrails

- Diff touches only files related to the task. Preserve existing formatting and lint rules.
- Names are intention-revealing.
- Comments (if any) explain business why, not how. Never excuse a workaround or restate a rule that belongs in lint or a test.
- Sanitize against SQLi, XSS, and command injection at trust boundaries. Never hardcode secrets.

## Project specifics

- API endpoint pattern: `<fill in>`
- Database access pattern: `<fill in>`
- Error handling pattern: `<fill in>`
- Logging pattern: `<fill in>`
- Feature / module structure: `<fill in>`
- Presentation / application / infrastructure layout: `<fill in>`
- Schema validator: `<fill in>`
- Structured log format: `<fill in>`
- Verify command: `<fill in>`
- Feature Map location: `<fill in>`

## Workflow

1. Audit the touched area for workarounds, duplicates, and dead code.
2. Name the layer and single pattern for endpoint, DB, error handling, logging, and module structure.
3. Build with no excuse-comments; encode rules in the API, lint, or tests.
4. Verify through the CLI and collect proof.
5. Add a lint/compiler/CI guard for any repeated mistake.
6. Update the Feature Map when user-facing behaviour changes.
