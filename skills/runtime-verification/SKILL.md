---
name: runtime-verification
description: Give the AI agent a way to run the app itself and prove a change works — one reusable CLI/driver instead of a fresh script per session, a Feature Map as shared vocabulary from the user's point of view, and video, screenshot, and state-chart evidence instead of reviewing diffs alone. Use when verifying a feature, fixing a UI bug, or reviewing agent work.
license: MIT
compatibility: Works with Claude Code, Cursor, Codex, OpenCode, opencode, Muse, and any agent that reads Agent Skills (SKILL.md).
metadata:
  author: gygabyte.lab
  version: "1.0.0"
  source: Lauren Tan (@poteto), "All Code Is Slop (Except Mine)", 14 Nov 2026, Grok Bot team
  tags: ["verification", "testing", "runtime", "feature-map", "ai-coding"]
---

# Runtime Verification

Source: Lauren Tan (@poteto), "All Code Is Slop (Except Mine)", 14 Nov 2026.
The Grok Bot team shipped 11,000 pull requests in a month with a revert
rate around 0.5%. Their review process shrank into looking at verification
artefacts — videos, screenshots, diagrams, formal verification — instead of
diffs. Around 70% of daily tokens go into verification (reported by Cindy,
confirmed by Lauren as matching her experience).

Use this skill when verifying a feature, fixing a UI bug, or reviewing agent
work. Apply it before presenting a change as done — a change without
evidence is a claim, not a result.

## 1. Principle — verification is what makes agents trustworthy

The missing ingredient for trusting agents is verification. Without it,
subtle bugs get merged and confidence collapses. With it, agents become
more autonomous: they can check their own work, and a human can review
artefacts instead of reading every diff.

Do not review code alone. Review the proof that the code does what it
claims to do, in the terms the user experiences.

## 2. Run it yourself — give the agent the same tools as a human

Runtime verification, in its most basic form, is giving the agent a way to
run the code itself and use the same debugging tools human developers use —
for example, the Chrome DevTools Protocol for a web or Electron app.

Requirements:

- The agent can **start the app**, navigate to the changed feature, and
  interact with it the way a user would.
- The agent can **inspect runtime state** — console errors, network
  requests, performance traces — with the same tools a developer would use.
- The agent can **reproduce the bug** before fixing it, and **re-run the
  same reproduction** after the fix. A fix that was never reproduced
  before and after is unverified.

If the agent cannot run the app, verification is limited to reading code —
and reading code is exactly what agents are worst at trusting.

## 3. One reusable CLI/driver — same command every time

Do not let the agent write a fresh test, run, or debug script every
session. Results and verification then vary between sessions, and the
verification itself cannot be tested.

Build or reuse one CLI/driver the agent uses to:

- Run the app
- Control the app (navigate, click, type)
- Debug it (logs, traces, console output)
- Take a performance trace
- Collect proof that a change actually works

Rules:

- **Same CLI, every time.** If the task needs a new capability, add it to
  the CLI — do not write a one-off script beside it.
- **Testable.** The CLI itself must be tested, so a passing verification
  run means something.
- **Token-efficient.** Reusing one driver cuts token usage compared with
  inventing commands per session.

If no such CLI exists for the project, build that first (or propose it as
the first task) instead of writing ad-hoc scripts per feature.

## 4. Feature Map — shared vocabulary from the user's point of view

A Feature Map is a site map of all features available in the application
from the user's point of view. It gives the agent a shared vocabulary, so
it does not have to guess "the thingy in the left sidebar".

Each entry covers:

- Name and function of the feature
- How the user reaches it
- Keyboard shortcuts
- DOM elements / selectors used
- Interactions to perform
- Expected result

Use the Feature Map when investigating a bug, especially when the report
is a vague screenshot: point the agent at the exact feature entry instead
of letting it guess from the UI. Update the map when a feature is added,
renamed, or moved — a stale map pointing at the wrong selector is worse
than no map.

This skill is standalone: it does not require another skill in this repo.
If the project already keeps a Feature Map (for example, following the
`ai-assisted-coding` template), use that map — do not create a second one.

## 5. Evidence — video, screenshots, and state charts

A change is done when the evidence exists, not when the diff looks right.

- **UI change:** a video or screenshot showing the feature working, before
  and after where the change is visual. What counts as proof for each kind
  of change is listed in `references/evidence-checklist.md`.
- **Bug fix:** reproduction of the bug before the fix, and the same
  reproduction passing after the fix.
- **Behaviour change:** a state chart visualising the state machine from
  the user's point of view — much easier to review than a diff, and it
  checks whether the agent understood the intent, not just the code.
  Lauren built a "state-chart" skill for exactly this: visualise behaviour
  in user terms alongside videos and screenshots.

Review the artefacts in user terms. If a human cannot tell from the
evidence whether the feature does what the user wants, the verification
is incomplete — no matter how clean the diff is.

## 6. Budget — most tokens go to verification

Verification is the main token spend, not an afterthought. Around 70% of
daily tokens go into verification in Lauren's team, and she says that
tracks with her experience: most of her token usage goes to verification.

These techniques are not new — they are good software engineering tools
that were previously prohibitively expensive to understand, write, or
maintain. Agents drastically cut that cost, so spending tokens on
verification is now the cheap way to buy trust.

Practical consequences:

- Budget verification time and tokens **first** when planning a task, not
  from whatever is left after writing code.
- A task that "saves tokens" by skipping verification is not cheaper — it
  moves the cost to review, reverts, and bug fixes.
- If verification is expensive for a particular change, that is a signal
  the change needs a better CLI, a better Feature Map entry, or a smaller
  scope — not a reason to skip it.

## 7. Workflow + checklist

For every feature, fix, or agent-produced change:

1. **Reproduce first.** Before changing code, reproduce the bug or
   demonstrate the missing behaviour with the CLI, and record the
   evidence.
2. **Locate via the Feature Map.** Point the agent at the exact feature
   entry — name, path, selectors, expected result.
3. **Change.** Write the code (see `enforceable-architecture` and
   `core-engineering-standards` for how the code itself should be
   written).
4. **Run it yourself.** Run the app through the reusable CLI and exercise
   the changed feature as a user would.
5. **Collect evidence.** Video/screenshot for UI changes, reproduction
   before/after for bug fixes, state chart for behaviour changes. Check
   against `references/evidence-checklist.md`.
6. **Review artefacts, not diffs.** The human reviews the evidence in
   user terms; the diff is supporting material.
7. **Harden if it failed.** If verification caught a repeated mistake,
   turn it into a deterministic check (lint / compiler / CI) before
   closing the task.

A task is not done until steps 4–6 are complete.

## Project rules template

If the project has no AGENTS.md / CLAUDE.md yet, copy `assets/AGENTS.md`
into the project root and fill in the project-specific verify command and
Feature Map location. That file is the portable version of this skill for
agents that do not load skills.
