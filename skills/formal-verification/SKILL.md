---
name: formal-verification
description: Add a formal-verification layer on top of tests — model-check the most important stateful flows and prove key algorithms — as one more layer alongside runtime verification, not a silver bullet. Use when a flow spans devices or platforms, an algorithm must not flicker or jitter, or a UI has stateful edge cases.
license: MIT
compatibility: Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.0.0"
  source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026
  tags: ["formal-verification", "tla-plus", "lean", "property-based-testing", "model-checking", "testing"]
---

# Formal Verification

Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026. At Grok Bot, formal verification is a promising additional layer on top of tests and runtime verification — it has already found quite a few bugs — but it is still being experimented with, not a silver bullet.

Use this skill when a bug would be expensive: a flow that must hold across devices or platforms, an algorithm whose correctness is hard to see by testing alone, or a UI with stateful edge cases. Do not start here for ordinary code — plain tests and runtime verification come first.

## 1. Principle — check properties, not just examples

- Tests check examples: given this input, expect that output.
- Formal verification checks properties and invariants: the property must hold for every reachable state, not just the cases someone wrote down.
- It also checks whether the agent understood intent. Writing the property forces the intent to be stated precisely enough to be checked — that alone is valuable, even before a bug is found.
- It is one layer in the agent environment, alongside the codebase, static analysis, and runtime verification — not a replacement for any of them.

## 2. TLA+ — model-check the most important stateful flows

- Use TLA+ for model checking major user flows, especially flows that span multiple devices and platforms.
- Example from Grok Bot: notifications across multiple devices and platforms.
- Model the flow as states and transitions, state the invariants (e.g. a notification is never lost, never delivered twice, never shown in a stale state), and let the model checker explore the reachable states.
- Start with the most important stateful flows only. A flow earns TLA+ when a wrong state is expensive — data loss, duplicate side effects, or a user-visible contradiction across devices.

## 3. Lean — prove key algorithms

- Use Lean for proving certain algorithms — the code where correctness matters more than it can be shown by a handful of test cases.
- Example from Grok Bot: their own custom virtualisation library, checked so it does not have flickering or jittering problems.
- A proof earns its cost when the algorithm is small, central, and hard to test exhaustively — a virtual list, a scheduler, a layout calculation, a sync or merge algorithm.
- Do not try to prove everything. Prove the algorithm that everything else depends on.

## 4. Property-based UI testing — catch what example tests miss

- Use property-based testing for UIs: state a property the UI must always satisfy, then generate many inputs and check the property holds for all of them.
- Example from Grok Bot: the Bombadil library for property-based UI testing, which has already caught a few UI bugs.
- This is the cheapest entry point of the three: it needs a property and a generator, not a full model or a proof.
- Use it when a UI has stateful edge cases — lists that grow and shrink, forms with interdependent fields, views that update from several sources at once.

## 5. Where to start

1. List the stateful flows and algorithms where a bug is expensive.
2. Rank them by cost of a bug, not by how interesting the technique is.
3. Start with property-based testing on the UI edge cases — cheapest, fastest feedback.
4. Add TLA+ for the one or two most important cross-device or cross-platform flows.
5. Add Lean for the one algorithm that everything else depends on.
6. Keep runtime verification and ordinary tests in place. Formal verification layers on top of them; it does not replace them.

See `references/when-to-use.md` for the decision guide: TLA+ vs Lean vs property-based testing vs plain tests.

## 6. Limits

- Cost and learning curve are real. TLA+ and Lean both require learning a new way of stating what the code should do, and that work was previously prohibitively expensive to write and maintain — agents cut that cost, but do not pretend it is zero.
- It is still being experimented with, even at Grok Bot. Treat a first formal-verification attempt as an experiment with a clear question: which property are we checking, and what bug would it have caught?
- A model or proof is only as good as the property written into it. A wrong property, proved, is worse than no property — it gives false confidence.
- If the property cannot be stated precisely, that is a signal the intent itself is unclear. Clarify the intent first; do not paper over it with a proof.

## 7. Checklist

- [ ] The flow or algorithm is named, and the cost of a bug in it is stated.
- [ ] The property or invariant is written down in plain language before any model, proof, or test is written.
- [ ] The right tool is chosen: TLA+ for a stateful cross-device flow, Lean for a key algorithm, property-based testing for stateful UI edge cases, plain tests for everything else.
- [ ] Runtime verification and ordinary tests are still in place — this layer is added, not substituted.
- [ ] The bugs found (if any) are recorded, so the next decision about where to apply formal verification has evidence behind it.
- [ ] If no bug is found and the property was hard to state, the unclear intent is flagged to the user instead of being treated as verified.
