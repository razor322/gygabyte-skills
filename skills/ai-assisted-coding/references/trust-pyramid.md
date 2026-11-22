# Trust Pyramid

Source: Lauren Tan, "All Code Is Slop (Except Mine)" (14 Nov 2026), 2:03.
The pyramid is inverted: the largest, most deterministic layers are at the
bottom, the smallest layer — prompts — is at the top.

## The four layers

1. **The codebase itself.** Good API design, architecture, and refactoring.
   This is the biggest investment. A clean codebase teaches the agent good
   patterns; a messy one teaches bad ones, because the agent copies whatever
   is already there. Lauren's image: the codebase is a garden — guardrails
   are the concrete foundations new code grows around, while workarounds,
   anti-patterns, deprecated code, and tech debt are weeds that spread
   because agents copy them as if they were the established pattern.
2. **Static analysis.** Lint rules, type systems, compilers, testing.
   Together with layer 1, these constrain what the agent can do and force
   the right thing by default, without anyone having to remember a rule.
3. **Verification.** Runtime and formal verification, to find bugs, check
   properties and invariants, and check whether the agent understood the
   intent behind the task.
4. **Prompts.** Rules, skills, `agents.md`. The smallest layer, because it
   requires judgement to apply and cannot be enforced mechanically.

## The correction rule

When correcting the agent, ask first: *how do I turn this correction into
a deterministic check?* Only if that is impossible should the correction
become a line in a prompt, rule, or skill file. Code is far more
reproducible and trustworthy than prose — a lint rule fires every time,
a sentence in `agents.md` is read, interpreted, and sometimes ignored.

This is why section 3 of the skill prefers, in order: lint rule, then
compiler/type check, then CI validation, and only then a recorded rule in
the project's rules file.

## Why the order matters

Lauren reviewed about 3 PRs a day from colleagues in 2024; now it is more
like 300. Every engineer is effectively a team of hundreds or even
thousands of agents, and code arrives as a continuous stream. At that
volume, judgement-based review of every diff does not scale — the only
things that scale are the deterministic layers underneath: a codebase the
agent cannot easily misuse, static checks that catch misuse, and
verification that proves the result.

## Related rules in this skill

- Section 2: encode constraints in the API/architecture, rules in lint,
  business rules in tests — never in a comment.
- Section 3: every repeated AI mistake becomes a lint/compiler/CI rule.
- Section 8: spend most tokens on verification, and review artefacts
  (video, screenshots, state charts), not just diffs.
