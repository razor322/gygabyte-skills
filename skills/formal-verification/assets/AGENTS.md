# Formal Verification Rules

Rules for any agent working in this project. Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026.

## 1. Properties before tools

State the property in plain language first: what must always hold, and what a bug here costs. If the property cannot be stated precisely, the intent is unclear — clarify it before modelling or proving anything.

## 2. Pick the right tool

- Stateful flow across devices or platforms (e.g. notifications) → TLA+ model checking.
- Small, central algorithm that must be correct (e.g. virtualisation, no flicker/jitter) → Lean proof.
- Stateful UI edge cases → property-based testing (e.g. Bombadil).
- Ordinary logic with a clear contract → plain tests. Formal verification is overkill.

## 3. Start where a bug is expensive

Rank flows and algorithms by the cost of a bug, not by how interesting the technique is. Start with property-based testing (cheapest), then TLA+ for the most important flows, then Lean for the one algorithm everything depends on.

## 4. Layer on, don't replace

Formal verification sits on top of tests and runtime verification. It does not replace them. Keep running the app and checking it like a user.

## 5. A wrong property is worse than none

A model or proof is only as good as the property written into it. A wrong property, proved, gives false confidence. If no bug is found and the property was hard to state, flag the unclear intent instead of treating the code as verified.

## 6. Record what it found

Record the bugs formal verification finds (or that it found none). The next decision about where to apply it needs that evidence. At Grok Bot it has already found quite a few bugs — but it is still experimental, not a silver bullet.
