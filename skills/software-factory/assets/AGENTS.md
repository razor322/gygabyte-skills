# Software Factory Rules

Rules for any agent working in this project. Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026.

## 1. Foundations before factories

Fix the inner coding loop first. Automating a loop that produces slop just produces more slop and burns tokens.

## 2. Correct via the codebase, in order

1. Lint rule first — prevent new occurrences of the anti-pattern.
2. Clean up the old patterns too.
3. Make verification strong.
4. Only then build the outer loop.

## 3. Do not automate yet if

- Revert rate is high or rising with volume.
- Verification is weak — no way for the agent to run the app and prove a change works.
- PRs still need deep human review.
- The codebase still teaches bad patterns.

Fix the foundation first.

## 4. Judge by revert rate, not PR count

Track revert rate and user-visible performance under volume. Reference: Grok Bot shipped 11,000 PRs a month with a revert rate steady at about 0.5%, and a new stable version about once a day.

## 5. Outer loop: bounded and event-triggered

Automation is triggered by an event (e.g. a release), and each job is bounded: fuzz the app like a real user, one bug, one fix PR, automerge only when PRs are good by default.

## 6. Solo-dev version

One verify + fuzz command after each release, fix by hand, no automerge yet. Automerge is the last step, not the first.
