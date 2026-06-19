---
name: fix
description: Document an ad-hoc bug fix or small change (one not in the build plan) into context/current-feature.md so it runs through the same build loop. Writes a short fix spec and stops; then /implement builds it and /complete logs it to docs/fixes/ and merges. Use when the user runs /fix, reports a bug, or asks to fix or change something that isn't a planned build-plan feature.
---

# fix - document an ad-hoc fix, then build it like anything else

Where this sits in the workflow:

    /fix  ->  /implement  ->  /complete  ->  back to your features
    (spec     (build it,      (log to docs/fixes/
     the fix)  reviewed)       + merge)

A fix is a bug or small change that isn't a planned build-plan feature. It runs
through the same loop as a feature (build with review gates, iterate, then merge);
it just starts here instead of `/feature`, and is logged separately.

## Input

A description of the bug or change, for example `/fix "password reset email never
sends"`. If the user just reported the problem in chat, use that.

## Step 1 - write the fix spec

Pull context from `context/project-overview.md` and `context/coding-standards.md`,
then write a short spec to `context/current-feature.md` (this file holds whatever
is being built now, feature or fix). Keep it lighter than a feature spec:

- **Title** - the bug or change in a few words.
- **Type:** Fix  (so `/complete` logs it to `docs/fixes/`, not `docs/features/`).
- **The problem** - what's wrong or what needs to change, and where.
- **The fix** - the approach, and anything it must not break.
- **Build steps** - usually one small step; split only if the diff would be too
  big to read. Each ends with an observable "done when".
- **Verify** - how to confirm it's fixed (what to click or test).

Then stop. Tell the user to review the fix spec, then run `/implement` to build it.

## Rules

- A fix is not a build-plan item; don't add it to `build-plan.md`.
- Keep it small. If it's really a new feature, use `/feature` and the build plan
  instead.
- Same conventions as everything else (`context/coding-standards.md`).
