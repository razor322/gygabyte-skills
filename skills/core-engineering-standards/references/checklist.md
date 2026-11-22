# Pre-submit checklist — Core Engineering Standards

Run through this list before presenting code as done. Any "no" is either a fix or a flag to the user.

- [ ] Every new function is 30–40 lines or fewer and has one reason to change.
- [ ] Nesting is at most 2 levels; edge cases are handled by guard clauses at the top.
- [ ] Types are explicit at every boundary; no `any` / untyped map without a stated reason.
- [ ] No magic strings, status codes, or config values inline — all extracted to constants / enums.
- [ ] No raw DB query, network call, or heavy business logic in a UI component / controller.
- [ ] Files approaching 200–250 lines are split into focused modules/components.
- [ ] No empty `catch` / `except`, no silently swallowed error.
- [ ] Error logs are structured for developers; client-facing messages are sanitized and user-friendly.
- [ ] All external inputs (payloads, query params, uploads) are validated with a schema validator.
- [ ] Diff touches only files related to the task; formatting and lint rules of the codebase are preserved.
- [ ] Names are intention-revealing; comments (if any) explain a business *why*, not a *how*.
- [ ] Inputs are sanitized against SQL injection, XSS, and command injection; no hardcoded secrets.
