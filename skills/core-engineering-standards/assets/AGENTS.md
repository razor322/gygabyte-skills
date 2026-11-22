# Core Engineering Standards

Rules for any agent working in this project. Fill in the placeholders with this project's actual standards.

## Clean and idiomatic code

- Follow the idiomatic style of the language and framework. No personal style on top of conventions.
- One reason to change per module, class, and function.
- Functions under 30-40 lines. Guard clauses and early returns; nesting at most 2 levels.
- Explicit types at every boundary. Avoid `any`, `interface{}` without type assertions, and untyped maps/dictionaries unless strictly required. State the reason in the reply, not in an excuse-comment.
- Constants, status codes, magic strings, and config values live in dedicated constant files or enums. No hardcoded values.

## Architecture

- Presentation: UI, pages, controllers — render data, translate input.
- Application: use cases, orchestration, business rules.
- Infrastructure: database, API clients, caching, third-party integrations.
- No raw DB query, network call, or heavy business logic in UI components / controllers.
- Split files approaching 200-250 lines into focused modules.

## Enforceable architecture

- Encode hard-won lessons as constraints, not advice. Anything that needs nuance to use well tends to produce slop.
- Ban nuance-heavy APIs in product code and provide a safe wrapper instead. Example: React `useEffect` banned in product code, allowed only inside the framework APIs (Grok Bot/Dune).
- Make the correct structure obvious through directory names, then enforce the boundary mechanically. Example: `electron/main` vs `electron/renderer` as top-level directories, imports between them forbidden by a dependency-graph analyser + lint.
- Layer boundaries are enforced by lint / dependency analysis / the module system, not by convention alone. A lint error must name the safe alternative.

## Error handling

- No empty catch/except, no swallowed errors.
- Structured error logs for developers; sanitized, user-friendly messages for clients.
- Validate all external inputs (payloads, query params, uploads) with a schema validator before domain logic.

## Refactoring guardrails

- Diff touches only files related to the task. Preserve existing formatting and lint rules.
- Names are intention-revealing.
- Comments (if any) explain business why, not how. Never excuse a workaround or restate a rule that belongs in lint or a test.
- Sanitize against SQLi, XSS, and command injection at trust boundaries. Never hardcode secrets.

## Project specifics

- Presentation pattern: `<fill in>`
- Application / use-case pattern: `<fill in>`
- Infrastructure pattern: `<fill in>`
- Schema validator: `<fill in>`
- Structured log format: `<fill in>`
- Verify command: `<fill in>`
