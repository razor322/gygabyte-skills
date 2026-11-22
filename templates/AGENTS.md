# Project Rules — Gygabyte Skills

Rules for any agent working in this project. Fill in the placeholders with this project's actual standards. Copy this file to the project root as `AGENTS.md` or `CLAUDE.md`.

## Trust pyramid — spend time where it is deterministic

Spend the most effort at the bottom, the least at the top:

1. Codebase itself — API design, architecture, refactoring.
2. Static analysis — lint rules, type system, compiler, tests.
3. Verification — runtime and formal; checks properties, invariants, and whether the agent understood the intent.
4. Prompts — rules, skills, `AGENTS.md`. Smallest layer; needs judgement.

When correcting the agent, first ask how to turn the correction into a deterministic check, not another line in the rules file. Spend most tokens on verification, not on writing code. Judge an agent system by revert rate and user-visible performance, not by PR count.

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

### Review artefacts, not diffs

Require video/screenshot evidence (and a state chart of the behaviour from the user's point of view) before calling a change done. Review the artefacts to check intent, not only the diff.

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

### Enforceable architecture

- Encode hard-won lessons as constraints, not advice. A rule that lives only in a comment or in convention will be ignored or copied wrong.
- Ban nuance-heavy APIs in product code and provide a safe wrapper instead; the raw API lives only inside the framework/API layer.
- Make the correct structure the obvious one through directory layout (e.g. `electron/main` vs `electron/renderer`), and forbid crossing the boundary with a dependency-graph analyser and lint rules.
- Layer boundaries are enforced mechanically by lint/dependency analysis, not by convention alone.

### Error handling

- No empty catch/except, no swallowed errors.
- Structured error logs for developers; sanitized, user-friendly messages for clients.
- Validate all external inputs (payloads, query params, uploads) with a schema validator before domain logic.

### Refactoring guardrails

- Diff touches only files related to the task. Preserve existing formatting and lint rules.
- Names are intention-revealing.
- Comments (if any) explain business why, not how. Never excuse a workaround or restate a rule that belongs in lint or a test.
- Sanitize against SQLi, XSS, and command injection at trust boundaries. Never hardcode secrets.

## Part 3 — Enforceable architecture

- Ban + safe wrapper: forbid the nuance-heavy API in product code; expose only the safe wrapper. The raw API is allowed only inside the framework/API layer, with a named owner.
- Structure as context: name directories so the correct structure is the obvious one; forbid cross-boundary imports mechanically.
- Lint errors must teach: a helpful error message points at the root cause and the safe alternative, not just "forbidden".
- Do not ban what has no safe alternative yet — build the wrapper first.

## Part 4 — Runtime verification

- The agent runs the app itself with the same debugging tools a human uses (e.g. Chrome DevTools Protocol) and reproduces the behaviour before and after the change.
- One reusable CLI/driver for running, controlling, debugging, and collecting proof — the same CLI every session, never a fresh ad-hoc script.
- Feature Map is the shared vocabulary: features named from the user's point of view, so the agent stops guessing.
- Proof for a change is video/screenshot (plus a state chart for behaviour); a passing test alone is not proof for a UI change.
- Most tokens go to verification, not to writing code.

## Part 5 — Formal verification

- Layer formal verification on the most important stateful flows and algorithms first; it is one more layer alongside runtime verification and tests, not a silver bullet.
- TLA+: model-check major user flows (e.g. notifications across devices/platforms).
- Lean: prove key algorithms (e.g. a custom virtualisation library, so it does not flicker/jitter).
- Property-based UI testing catches stateful edge cases plain tests miss.
- Only worth it where a bug is expensive; start with the flow or algorithm that matters most.

## Part 6 — Software factory

- Foundations before factories: fix the inner coding loop first. Automating a slop-producing loop produces more slop and burns tokens.
- Order: correct via the codebase first; lint rule first, then clean up legacy patterns; make verification strong; only then build the outer loop.
- Outer loop example: on each new release, spin up a fuzzing run that uses the app like a real user, opens a fix PR per bug, and automerges — safe only when PRs are good by default and the revert rate is low and stable.
- Judge by revert rate and user-visible performance, not by PR count.
- Solo-dev version: one verify + fuzz command after each release, before attempting auto-fix or automerge.

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
- Banned-in-product-code APIs and their safe wrappers: `<fill in>`
- Layer-boundary rules (directories + lint): `<fill in>`
- Runtime verification CLI/driver: `<fill in>`
- Formal-verification targets (flows/algorithms): `<fill in>`
- Release fuzz/automation trigger: `<fill in>`

## Workflow

1. Audit the touched area for workarounds, duplicates, and dead code.
2. Name the layer and single pattern for endpoint, DB, error handling, logging, and module structure.
3. Build with no excuse-comments; encode rules in the API, lint, or tests.
4. Verify through the CLI and collect proof (video/screenshot/state chart where it applies).
5. Add a lint/compiler/CI guard for any repeated mistake; if a pattern keeps being misused, turn it into an enforceable constraint (Part 3).
6. Update the Feature Map when user-facing behaviour changes.
7. Before automating anything (Part 6), check the readiness gates: clean codebase, strong verification, low and stable revert rate.
