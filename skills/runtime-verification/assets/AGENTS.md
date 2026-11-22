# Runtime Verification Rules

Rules for any agent working in this project. Fill in the placeholders with this project's actual verify command and Feature Map location.

## 1. Verify, don't claim

A change is done when the evidence exists, not when the diff looks right. Review artefacts (video, screenshots, state charts), not diffs alone.

## 2. Run it yourself

Verify command (one reusable CLI/driver): `<fill in>`

Use it to run the app, control it, debug it, take a performance trace, and collect proof. The agent must run the app with the same debugging tools a human developer uses. Do not write a throwaway script per session — add missing capability to the CLI instead.

## 3. Reproduce before and after

For a bug fix: reproduce the bug before the fix, then run the same reproduction after the fix. Record the command and the evidence. A fix that was never reproduced is unverified.

## 4. Feature Map

Map location: `<fill in>`

Point the agent at the exact Feature Map entry (name, path, selectors, expected result) instead of letting it guess from a vague screenshot. Update the map when a feature is added, renamed, or moved.

## 5. Evidence

Minimum evidence per change (see `references/evidence-checklist.md` in the skill):

- UI change → video/screenshot in the running app.
- API change → CLI run with request/response + error cases.
- Bug fix → reproduction before + after, same command.
- Behaviour change → state chart + video/screenshot.
- Performance change → before/after trace through the CLI.

## 6. Budget

Most tokens go to verification — it is the main spend, not an afterthought. Budget verification first when planning a task. Skipping verification does not save cost; it moves it to review, reverts, and bug fixes.

## Workflow

1. Reproduce first (bug) or demonstrate the missing behaviour.
2. Locate via the Feature Map.
3. Change the code.
4. Run it yourself through the CLI, as a user would.
5. Collect evidence per the checklist.
6. Human reviews artefacts in user terms; the diff is supporting material.
7. If verification caught a repeated mistake, add a lint/compiler/CI rule before closing.
