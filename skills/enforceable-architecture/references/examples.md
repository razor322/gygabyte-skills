# Examples — Enforceable Architecture

Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026. All
examples below are from the talk, plus one generic worked example built
from the same pattern. No facts beyond the talk are claimed.

## Example 1 — Ban `useEffect` in product code (Dune / Grok Bot)

**The problem.** React `useEffect` has legitimate uses, but it is often
misused — Lauren's words: "you need a PhD to use useEffect" — and agents
are not good at it. The correct use depends on nuance: dependencies,
cleanup, ordering, and when an effect is really a derived value or an
event handler in disguise.

**The cost.** Misused effects cause stale data, double runs, and bugs that
are hard to catch in review, because each use looks plausible on its own.

**The constraint (Dune).**

1. **Ban:** `useEffect` is forbidden in product code, enforced by a lint
   rule. The lint error tells the developer/agent what to use instead.
2. **Wrapper:** the framework layer (Dune APIs) provides safe alternatives
   that cover the legitimate cases — data fetching, subscriptions, and
   similar — with the dangerous decisions already made inside the API.
3. **Escape hatch:** `useEffect` is allowed only in some places inside
   Dune APIs themselves, owned by the framework team. Product code never
   touches it directly.

**The result.** The problem is categorically eliminated. It never needs to
be caught in code review again, because the forbidden code cannot be
written in product code in the first place.

**What not to do.** Do not replace the ban with a comment ("avoid
useEffect here") or a rule in a doc ("prefer X over useEffect"). Comments
and markdown are not enforceable, go stale, and give an agent an escape
hatch to justify the misuse.

## Example 2 — `electron/main` vs `electron/renderer` (Cursor → Dune)

**The problem.** Cursor is an Electron app. Agents often imported
main-thread code into the renderer thread, causing long tasks and jank.
The distinction between the two threads is a judgement call if it lives
only in documentation — and agents do not reliably make that call.

**The cost.** Long tasks on the renderer thread block the UI: lag, jank,
and a visibly worse app, caused by an import that looked harmless.

**The constraint (Dune).**

1. **Structure as context:** two top-level directories literally called
   `electron/renderer` and `electron/main`. The correct placement is
   obvious from the tree — no document needed to know which side a file
   belongs to.
2. **Mechanical enforcement:** imports between the two directories are
   forbidden, banned by a dependency-graph analyser and lint rules. The
   check fails the build/lint; it does not warn.
3. **No convention-only boundary:** the boundary is not a naming
   convention or a comment. It is a check that runs on every change.

**The result.** An agent placing a new file reads the tree, puts it on
the right side, and cannot accidentally import across the boundary —
the tooling stops it with an error before the code is ever reviewed.

## Example 3 — Generic worked example: imperative timers

Use this template to convert any recurring lesson. The facts of the
timer case are from the talk (Cursor); the worked steps are the generic
pattern.

**The problem.** In Cursor, agents loved imperative timers — `setTimeout`,
`requestAnimationFrame`, `requestIdleCallback` — as quick fixes patching
an architecture clash between VS Code's imperative style and React's
declarative style. Many timers rapidly created and destroyed cause major
garbage collection, lag, and jank. Worse, agents wrote many comments
explaining why the quick fix was justified, and timers spread everywhere.

**Apply the checklist (SKILL.md §6):**

1. **Name the lesson:** imperative timers as a patch for a declarative
   architecture clash; cost is GC pressure, lag, and jank.
2. **Eliminate categorically?** Ban the three timer APIs in product code
   and provide a declarative wrapper (for example, a framework-provided
   scheduling API) that covers the legitimate cases. If a wrapper cannot
   cover a case, that case belongs in the framework layer with a named
   owner.
3. **Structure?** Put the wrapper in the framework layer's API directory
   so the safe path is the one an agent finds first.
4. **Check mechanically?** Lint rule banning the raw timer APIs in
   product code, with an error message that names the wrapper and says
   to fix the root cause rather than patch around it.
5. **Clean up old patterns:** remove existing timer usages and the
   comments justifying them — the comments are part of the problem,
   because they teach the next agent that the workaround is acceptable.
6. **Verify the constraint:** write the forbidden timer code and confirm
   the lint fails with a useful message; write the wrapper version and
   confirm it passes.
7. **Delete the advice:** remove the doc paragraph or comment that said
   "avoid timers" — the lint rule is now the source of truth.

**The test for any example.** If the same mistake can still be made by
an agent that never read the docs, the lesson has not been encoded yet.
