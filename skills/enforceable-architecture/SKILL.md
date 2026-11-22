---
name: enforceable-architecture
description: Turn hard-won lessons into enforceable architecture constraints instead of advice — ban nuance-heavy APIs in product code and provide a safe wrapper, make the correct structure obvious through directory layout, and enforce layer boundaries mechanically with lint and dependency analysis. Use when a pattern keeps being misused, an agent keeps crossing a layer boundary, or a rule keeps being ignored.
license: MIT
compatibility: Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.0.0"
  source: Lauren Tan (@poteto), "All Code Is Slop (Except Mine)", 14 Nov 2026, Dune framework for Grok Bot
  tags: ["architecture", "lint", "constraints", "code-quality", "ai-coding"]
---

# Enforceable Architecture

Source: Lauren Tan (@poteto), "All Code Is Slop (Except Mine)", 14 Nov 2026.
Dune is the client framework built for Grok Bot — "kind of like Next.js for
Electron apps" — not very interesting technically, but full of constraints and
lint rules encoding lessons from building desktop apps. The Grok Bot team
shipped 11,000 pull requests in a month with a revert rate around 0.5%.

Use this skill when a pattern keeps being misused, an agent keeps crossing a
layer boundary, or a rule keeps being ignored despite being written down.
Apply it before adding another rule to a doc — a constraint in code replaces
a paragraph of advice.

## 1. The principle — encode it, don't advise it

Anything that requires nuance to use well often leads to slop. Advice,
comments, and markdown rules are not enforceable: they go stale, are rarely
read, and give an agent an escape hatch to justify not doing the right thing.

When a lesson is learned the hard way, encode it as architecture:

- A ban in product code, with a safe wrapper provided.
- A directory layout that makes the right structure the obvious one.
- A boundary enforced by a dependency analyser or lint rule.

If the lesson lives only in a document, the next agent — and the next
developer — will repeat the mistake. If it lives in the architecture, the
mistake becomes impossible, or at least loud.

## 2. Ban + wrapper — remove the nuance-heavy API from product code

Some APIs have legitimate uses but are misused so often that the misuse is
the norm. Agents are especially bad at them, because the correct use depends
on judgement the agent does not have.

The pattern:

1. **Name the API and the misuse.** What does it get used for, and what goes
   wrong? (See `references/examples.md` for the `useEffect` case.)
2. **Ban it in product code** with a lint rule. The error message must say
   what to use instead — a ban with no alternative just pushes the agent to
   another workaround.
3. **Provide a safe wrapper** in the framework/API layer. The wrapper
   exposes only the safe subset of the behaviour, with the dangerous
   decisions already made.
4. **Allow the raw API only inside the framework/API layer**, where it is
   owned by people who understand the nuance. Product code never touches it
   directly.

Result: the problem is categorically eliminated. It never needs to be
caught in code review again, because it cannot be written in the first
place.

## 3. Structure as context — make the right structure obvious

Agents do not read architecture documents reliably; they read the directory
tree. Use directory names to carry the context that would otherwise live in
a rule.

- Give each layer or thread a top-level directory with a literal name, e.g.
  `electron/main` and `electron/renderer`.
- Put the code where its name says it belongs. If an agent has to guess
  which side a file belongs to, the structure has already failed.
- Never rely on a comment or a naming convention alone to mark a boundary
  that matters — a boundary that matters is a boundary that is enforced
  (see §4).

The test: a new agent, with no project history, should place a new file in
the right directory on the first try, just by reading the tree.

## 4. Mechanical enforcement — lint and dependency analysis, not convention

A constraint that is not checked is a suggestion. Enforce boundaries with
tools, not with trust:

- **Dependency-graph analyser / import rules:** forbid imports across a
  boundary (for example, renderer code importing main-thread code, which
  caused long tasks and jank in Cursor). The build or lint must fail, not
  warn.
- **Lint rules with helpful error messages:** the message must point at the
  root cause and the fix, not just say "forbidden". A good message tells
  the agent to fix the real problem instead of patching around it.
- **No enforcement by comment or convention.** If the only thing stopping
  the wrong import is a comment that says "do not import this here", the
  boundary does not exist.

Every time the team corrects an agent, the correction should turn into one
of these deterministic checks, or into a categorical elimination through
better API design or architecture. Repeated enough, there is less and less
to look for in review — eventually agents can merge most of their own PRs.

## 5. When NOT to ban — the escape hatch

A ban is a tool, not a reflex. Do not ban an API when:

- The misuse is rare and the correct use is genuinely needed in product
  code — in that case, document the correct use with a test, not a ban.
- There is no safe wrapper yet. Banning without an alternative creates
  workarounds, which are worse than the original misuse.
- The problem is a one-off, not a repeated pattern. A ban pays for itself
  only when the mistake recurs.

When an escape hatch is genuinely needed:

- It lives **inside the framework/API layer only**, never in product code.
- It has a **named owner** — one person or team accountable for that use.
- It is marked explicitly (for example, an allow-listed file or a named
  exemption), so it is visible in review and cannot spread by copying.

## 6. Checklist — convert a lesson into a constraint

Use this when a mistake recurs or a correction keeps being needed:

1. **Name the lesson:** what was misused, and what was the cost?
2. **Can it be eliminated categorically?** Ban the API in product code and
   provide a safe wrapper. If yes, do that first.
3. **Can structure carry it?** Rename or split directories so the right
   placement is obvious. If yes, do that too.
4. **Can it be checked mechanically?** Add a lint rule or dependency-graph
   rule with a helpful error message pointing at the root cause and the
   fix.
5. **Clean up the old patterns.** A ban on new occurrences is half the job;
   existing occurrences teach agents the old pattern by example.
6. **Verify the constraint works:** write the forbidden code and confirm
   the check fails with a useful message; write the correct code and
   confirm it passes.
7. **Delete the advice.** Remove the comment, rule, or doc paragraph the
   constraint replaces, so the codebase carries one source of truth.

Worked examples: see `references/examples.md`.

## Project rules template

If the project has no AGENTS.md / CLAUDE.md yet, copy `assets/AGENTS.md`
into the project root and fill in the project-specific bans, wrappers, and
boundaries. That file is the portable version of this skill for agents
that do not load skills.
