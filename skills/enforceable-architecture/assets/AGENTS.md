# Enforceable Architecture Rules

Rules for any agent working in this project. Fill in the placeholders with this project's actual bans, wrappers, and boundaries.

## 1. Encode lessons, don't advise them

Anything that needs nuance to use well produces slop. When a lesson is learned the hard way, encode it as architecture — a ban, a directory layout, or a checked boundary — not as a comment or a doc paragraph.

## 2. Ban + wrapper

Banned in product code (raw API allowed only inside the framework/API layer):

- `<API, e.g. useEffect>`: use `<safe wrapper>` instead.
- `<API>`: use `<safe wrapper>` instead.

Rules:

- A ban must name its alternative in the lint error message. A ban with no alternative creates workarounds.
- The raw API lives only in the framework/API layer, with a named owner.
- Never work around a ban with a comment justifying the exception.

## 3. Structure as context

Layer/thread boundaries (enforced by dependency-graph / import rules):

- `<directory A>`: `<what belongs here>` — must not import from `<directory B>`.
- `<directory B>`: `<what belongs here>` — must not import from `<directory A>`.

A new file should land in the right directory on the first try, just by reading the tree.

## 4. Mechanical enforcement

- Boundaries are enforced by lint / dependency analysis and fail the check — they are not conventions or comments.
- Lint error messages must point at the root cause and the fix, not just say "forbidden".
- Every correction of a repeated mistake becomes a deterministic check, or a categorical elimination through better API design.

## 5. Escape hatch

An escape hatch is allowed only inside the framework/API layer, with a named owner: `<owner>`. It is explicitly allow-listed and visible in review. It never spreads into product code by copying.

## Checklist — convert a lesson into a constraint

1. Name the lesson and its cost.
2. Ban the API in product code; provide a safe wrapper.
3. Make the right structure obvious via directory layout.
4. Add a lint / dependency-graph rule with a helpful error message.
5. Clean up the old patterns (including the comments justifying them).
6. Verify: forbidden code fails with a useful message, correct code passes.
7. Delete the advice the constraint replaces.
