# Factory Readiness Checklist

Readiness gates to check before building the outer automation loop. Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026. Every gate must pass before automating; a failed gate means the next task is fixing the foundation, not building more automation.

## Gate 1 — Codebase is clean

- [ ] Known anti-patterns have lint rules preventing new occurrences.
- [ ] Old occurrences of those patterns have been cleaned up, not just fenced off.
- [ ] Workarounds, deprecated code, and duplicates in the touched areas are removed or explicitly named as the non-standard path.
- **Why:** agents copy what is in the codebase. Automating a codebase that teaches bad patterns multiplies the bad patterns.

## Gate 2 — Verification is strong

- [ ] The agent can run the app itself and prove a change works (runtime verification).
- [ ] Verification produces reviewable artefacts — video, screenshots, diagrams — not just a claim that it works.
- [ ] Ordinary tests and lint pass in CI before any PR is considered.
- **Why:** automerge is only safe when a bad PR is caught by checks instead of by a human reading the diff.

## Gate 3 — Revert rate is low and stable

- [ ] Revert rate is measured, not guessed.
- [ ] It is low and stable (reference: about 0.5% at Grok Bot, falling on a relative basis as velocity rose to 11,000 PRs a month).
- [ ] It does not rise as volume grows.
- **Why:** revert rate is the evidence the foundations work. PR count alone proves nothing.

## Gate 4 — PRs are good by default

- [ ] Most product code no longer needs deep human review.
- [ ] Review has shrunk to checking verification artefacts, not reading every diff.
- **Why:** the Grok Bot release-fuzzing loop automerges because PRs tend to be good by default. If a human must still read every diff carefully, automerge is not safe.

## Gate 5 — Performance holds under volume

- [ ] User-visible performance is tracked (startup, typing, opening and switching views — whatever the app's equivalents are).
- [ ] It is stable or improving as volume grows, not degrading.
- **Why:** velocity without quality is slop at scale. At Grok Bot, performance improved on multiple dimensions even as PR volume rose.

## Gate 6 — The outer loop is bounded and event-triggered

- [ ] Automation is triggered by an event (e.g. a new release is cut), not run by hand each time.
- [ ] Each job is bounded: one bug, one fix PR.
- [ ] Fuzzing uses the app like a real user, not just static checks.
- **Why:** unbounded automation burns tokens. Bounded, event-triggered jobs are checkable.

## Solo-dev version

If the gates above are not all met, the factory for one person is a single command run after each release:

1. Verify — full tests, lint, type check.
2. Fuzz — run the app and use it like a real user, or run an automated smoke pass over the main flows.
3. Fix by hand — one fix per bug found; no automerge yet.

Only when fix PRs are routinely good by default — low revert rate, verification catching the bad ones — consider automating the fix-and-merge step. Automerge is the last step, not the first.
