---
name: software-factory
description: Build the outer automation loop only after the inner coding loop is solid — release-triggered fuzzing, auto-fix PRs, and automerge — because automating a slop-producing loop just produces more slop and burns tokens. Use when setting up release automation or scaling agent output.
license: MIT
compatibility: Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.0.0"
  source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026
  tags: ["software-factory", "automation", "release", "fuzzing", "ci", "ai-coding"]
---

# Software Factory

Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026. At Grok Bot, the outer automation loop — release-triggered fuzzing, auto-fix PRs, automerge — works only because the inner coding loop was fixed first. Automating a loop that produces slop just produces more slop and burns more and more tokens, which is very expensive.

Use this skill when setting up release automation or scaling agent output. Do not use it to skip the foundations — if the foundations are weak, this skill tells you to fix them first.

## 1. Principle — foundations before factories

- Fix the inner coding loop first. If that loop produces slop, automating it produces more slop.
- The foundations are: a clean codebase, static analysis (lint rules, type systems, compilers), and strong verification (runtime and formal).
- Once those investments are made, many things become possible and a factory can easily be built on top. That work is a prerequisite to automation anyway.

## 2. The order — correct via the codebase first

When you correct your agent, improve the codebase first, in this order:

1. **Lint rule first** — add a lint rule to prevent new occurrences of the anti-pattern.
2. **Clean up the old patterns** — remove the existing occurrences as well, so agents stop copying them.
3. **Make verification strong** — runtime verification and formal verification, so bugs are caught by checks instead of by review.
4. **Only then build the outer loop** — release-triggered automation, auto-fix PRs, automerge.

Skipping steps 1–3 does not save time. It moves the cost to tokens burned on slop and to reverts.

## 3. The outer loop — orchestrate many agents

- The outer loop is connectors and routines that kick off cloud agents and projects, orchestrating hundreds or even thousands of agents.
- Example from Grok Bot: connectors that kick off Cursor cloud agents and projects.
- The outer loop is triggered by an event — a release, a schedule, a new version — not run by hand each time.
- Its jobs are bounded and repeatable: fuzz the app, find bugs, open a fix PR per bug, automerge. Each job is one small, checkable unit of work.

## 4. Worked example — fuzzing on every release

Whenever Grok Bot cuts a new release, it automatically spins up a new Cursor project that:

1. Fuzzes the entire application and build.
2. Finds bugs by running the app and using it like a real user.
3. Opens a fix PR for each bug.
4. Automerges the fix PR.

That sounds crazy, but it works because the codebase is good enough that PRs tend to be good by default and most product code no longer really requires deep human review. The automerge is safe only because the foundations make bad PRs rare — the factory did not create that safety, it inherits it.

## 5. When NOT to automate

Do not build the outer loop yet if any of these is true — fix the foundation first:

- Revert rate is high, or rising as volume grows.
- Verification is weak: no runtime verification, no reliable way for the agent to run the app and prove a change works.
- PRs still need deep human review. If a human must read every diff carefully, automerge is not safe.
- The codebase still teaches bad patterns: workarounds, deprecated code, and anti-patterns the agent will copy at scale.

Automating in that state burns tokens on slop and multiplies the review burden instead of removing it.

## 6. Metrics — judge by revert rate, not PR count

- Judge an agent system by revert rate and user-visible performance under volume, not by PR count alone.
- Reference numbers from Grok Bot: 11,000 PRs to production in a month, revert rate steady at about 0.5% (and falling on a relative basis as velocity rose), a new stable version cut about once a day, 227 people contributing at least one PR in October.
- Performance also improved on multiple dimensions compared with launch — startup, typing, opening new chats, and switching chats all got faster — even though Grok Bot is still an Electron app.
- Track before automating: revert rate, and user-visible performance. If either gets worse as volume grows, stop scaling and fix the foundation.

## 7. Solo-dev scaled-down version

A solo developer does not need hundreds of agents. The scaled-down factory is one command, run after each release, before attempting auto-fix or automerge:

1. **Verify** — run the full verification command (tests, lint, type check).
2. **Fuzz** — run the app and use it like a real user, or run an automated fuzz/smoke pass over the main flows.
3. **Fix by hand** — open one fix per bug found; do not automerge yet.
4. Only when fix PRs are routinely good by default — low revert rate, verification catches the bad ones — consider automating the fix-and-merge step.

One verify + fuzz command after each release is the factory for one person. Automerge is the last step, not the first.

See `references/factory-checklist.md` for the readiness gates to check before automating.

## 8. Checklist

- [ ] Inner loop is solid: clean codebase, lint rules prevent known anti-patterns, old patterns cleaned up.
- [ ] Verification is strong: the agent can run the app and prove a change works; runtime verification is in place.
- [ ] Revert rate is low and stable as volume grows (reference: ~0.5% at Grok Bot).
- [ ] PRs tend to be good by default — most product code no longer needs deep human review.
- [ ] The outer loop is event-triggered (e.g. on release) and each job is bounded: one bug, one fix PR.
- [ ] User-visible performance is tracked and not degrading under volume.
- [ ] Solo dev: a single verify + fuzz command exists and runs after each release before any auto-fix or automerge is attempted.
- [ ] If any gate fails, the next task is fixing the foundation — not building more automation.
