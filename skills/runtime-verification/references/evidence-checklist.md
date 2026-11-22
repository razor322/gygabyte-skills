# Evidence Checklist — Runtime Verification

Source: Lauren Tan, "All Code Is Slop (Except Mine)", 14 Nov 2026. Review
artefacts, not diffs: videos, screenshots, diagrams, and formal
verification. A change is done when the evidence exists, not when the
diff looks right.

Use this checklist at step 5–6 of the workflow in `SKILL.md`. If an item
does not apply to the change, mark it N/A and say why — do not silently
skip it.

## UI change

- [ ] Video or screenshot of the changed feature working in the running
      app (not a mockup, not a code snippet).
- [ ] Before/after pair where the change is visual.
- [ ] The feature reached through the same path a user takes, as listed
      in the Feature Map entry (name, path, selectors).
- [ ] No console errors introduced; console output captured from the run.
- [ ] State chart (if behaviour changed): state machine visualised from
      the user's point of view, so a human can review intent, not just
      code.
- [ ] Keyboard shortcuts and interactions from the Feature Map entry
      still work as expected.

## API / backend change

- [ ] The endpoint exercised through the reusable CLI/driver — same
      command as every other verification run, not a one-off script.
- [ ] Request and response captured (status, payload, error cases).
- [ ] Error cases verified: invalid input, missing resource, and the
      failure the change was meant to fix.
- [ ] No regression in the neighbouring endpoints covered by the same
      Feature Map entries.
- [ ] If the change alters user-facing behaviour, the Feature Map entry
      is updated.

## Bug fix

- [ ] Bug reproduced **before** the fix with the CLI, evidence recorded
      (video, screenshot, log, or failing output).
- [ ] Same reproduction run **after** the fix, showing the bug is gone.
- [ ] The reproduction is repeatable: same command, same steps, same
      expected result — recorded so the next session can run it.
- [ ] Root cause named. If the fix is a patch around the root cause
      rather than a fix of it, say so explicitly.
- [ ] If the bug was a repeated mistake, a deterministic check (lint /
      compiler / CI) was added before closing the task.

## Performance change

- [ ] Performance trace collected through the reusable CLI before and
      after (startup, typing, opening, switching — whichever the change
      affects).
- [ ] Trace shows the claimed improvement in user-visible terms, not
      only in a micro-benchmark.
- [ ] No regression in the other dimensions measured in the same run.

## What does NOT count as proof

- A diff that "looks right".
- A comment explaining why the change should work.
- A one-off script written for this session and never run again.
- A test that was written after the fix and only asserts the new code's
  own behaviour.
- A screenshot of code, a log without the reproduction steps, or a
  passing run whose command is not recorded.

## Minimum evidence per change

| Change type | Minimum evidence |
|---|---|
| UI change | Video/screenshot in the running app, via Feature Map path |
| API change | CLI run with request/response + error cases |
| Bug fix | Reproduction before + after, same command |
| Behaviour change | State chart + video/screenshot |
| Performance change | Before/after trace through the CLI |

If the minimum evidence cannot be produced, the task is not done — say
what is missing and what is needed to produce it, rather than presenting
the change as finished.
