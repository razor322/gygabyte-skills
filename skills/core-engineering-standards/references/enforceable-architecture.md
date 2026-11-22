# Enforceable Architecture

Source: Lauren Tan, "All Code Is Slop (Except Mine)" (14 Nov 2026), 5:00 —
the Dune framework for the Grok Bot client.

## The principle

Anything that requires nuance to use well often leads to slop. Advice in a
document does not stop an agent (or a tired human) from using a tricky API
the wrong way — the API gets used the wrong way again and again, and each
instance has to be caught in review. The fix is to encode the hard-won
lesson as a constraint the toolchain enforces, so the wrong use becomes
impossible or immediately visible.

## Pattern 1 — Ban the nuance-heavy API, provide a safe wrapper

- Identify the API that is easy to misuse and hard to review (it usually
  has subtle lifecycle, ordering, or cleanup rules).
- Forbid its direct use in product code with a lint rule.
- Allow it only inside the framework/library layer, behind a safe wrapper
  API that product code uses instead.

Worked example — React `useEffect` in Grok Bot/Dune: `useEffect` has
legitimate uses, but is often misused, and agents are not good at it.
Dune bans `useEffect` in product code; it is allowed only in some places
inside the Dune APIs themselves. The class of bug is categorically
eliminated and never needs catching in code review again.

When to use this pattern: the API is used often, misused often, and the
misuse is hard to spot in a diff. When *not* to use it: the API is rare
and its correct use is genuinely needed in product code — then a lint
rule with a helpful error message pointing at the safe alternative is
still better than a comment.

## Pattern 2 — Context through directory structure

- Make the correct structure the conventional, obvious one by naming
  directories after the boundary they represent.
- Enforce the boundary mechanically with a dependency-graph analyser
  and/or lint rules that forbid imports across it.

Worked example — Electron main vs renderer in Grok Bot/Dune: in Cursor,
agents often imported main-thread code into the renderer thread, causing
long tasks and jank. In Dune there are two top-level directories literally
called `electron/renderer` and `electron/main`, and imports between them
are forbidden. The directory names give the agent the context it needs,
and the analyser makes the boundary real.

Generalise this: whenever two parts of a codebase must not depend on each
other (presentation vs infrastructure, server-only vs client-only code,
feature A vs feature B), give each part its own directory and forbid
cross-imports with a tool, not with a convention written in a README.

## Pattern 3 — Lint errors that teach the fix

A ban without guidance just produces a different workaround. The lint
rule's error message should tell the agent (or developer) to fix the root
cause — name the safe wrapper or the correct directory — rather than
patching around it. This is the same rule as in `ai-assisted-coding`
section 2: a rule belongs in lint, with a helpful message, not in a
comment that justifies the workaround.

## Checklist before adding a constraint

- [ ] The misuse is a repeated, real problem — not a hypothetical one.
- [ ] The ban/wrapper removes a whole class of mistakes, not one instance.
- [ ] The safe alternative exists and is at least as easy to use as the
      banned API.
- [ ] The lint/analyser error message names the safe alternative.
- [ ] Existing occurrences of the banned pattern are cleaned up, so the
      agent has no bad example left to copy.
