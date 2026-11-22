# When to Use Formal Verification

Decision guide for `formal-verification`. Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026.

## The question first

Before choosing a tool, answer these two questions in plain language:

1. **What property must always hold?** (e.g. "a notification is never lost", "the list never flickers", "this field is never shown in a stale state")
2. **What does a bug here cost?** (data loss, duplicate side effect, user-visible contradiction, visual jitter — or just an annoyance?)

If the property cannot be stated precisely, the intent is unclear. Clarify the intent first. If the cost of a bug is low, stop here — plain tests are enough.

## Decision table

| Situation | Tool | Why |
|---|---|---|
| A user flow spans multiple devices or platforms, and states must agree (e.g. notifications across devices/platforms) | **TLA+** — model checking | The state space is large and cross-device; a model checker explores reachable states a human would not think to test |
| A small, central algorithm must be correct, and testing cannot cover its cases (e.g. a custom virtualisation library that must not flicker or jitter) | **Lean** — proof | The algorithm is the thing everything else depends on; a proof covers every case, not just the tested ones |
| A UI has stateful edge cases — lists that grow/shrink, interdependent fields, views updated from several sources | **Property-based testing** (e.g. Bombadil) | Cheapest entry point: a property plus a generator, no full model or proof needed; already caught UI bugs at Grok Bot |
| Ordinary logic with a clear input/output contract | **Plain tests** | Formal verification is overkill; tests plus runtime verification are the right layer |

## Worked examples from Grok Bot

- **Notifications across devices/platforms → TLA+.** The flow is stateful, spans devices and platforms, and a wrong state (lost or duplicated notification) is user-visible and expensive. Model-check it.
- **Custom virtualisation library → Lean.** One algorithm, central to rendering, where flicker or jitter would be visible on every scroll. Prove it.
- **UI edge cases → Bombadil.** Property-based testing for UIs, catching bugs that example-based tests missed.

## Order of adoption

1. Property-based testing first — cheapest, fastest feedback.
2. TLA+ next, for the one or two most important stateful flows.
3. Lean last, for the one algorithm that everything else depends on.
4. Never before runtime verification and ordinary tests are in place. Formal verification layers on top of them.

## When not to use it

- The property cannot be stated precisely — fix the intent first.
- The bug cost is low — plain tests are enough.
- Runtime verification is still weak — strengthen that first; formal verification does not substitute for running the app.
- The goal is "verify everything" — it is a targeted layer for the flows and algorithms where a bug is expensive, not a blanket.
