---
name: check
description: Prove the current work actually does what its spec says by running the real app and observing behavior against the "done when" criteria in current-feature.md. Drives the app (browser, CLI, or server), captures evidence (screenshots, output, console/network errors), and reports pass/fail per criterion. Does not edit source or commit - it observes; fixing stays /implement's job. Use when the user runs /check, asks to confirm a step or feature works, wants proof before /complete, or wants to check a change in the running app rather than just the build. (Supersedes the built-in /verify with a spec-aware version inside blueprint projects.)
---

# check - prove it works against the spec, with evidence

Where this sits in the workflow:

    /implement  ->  [check]  ->  /complete
    (built a       (run the app,    (only once the
     step or        prove each       done-whens are
     the feature)   done-when)       proven)

`/implement` builds and does a quick build-plus-screenshot check inline. `/check`
is the deeper, repeatable gate for when a "done when" needs the *real running app*,
not just a green build: a click that triggers a download, a route that returns a
file, a flow across screens. Run it on a single step whose done-when is
behavioral, or on the whole feature as the acceptance check before `/complete`.

The point is evidence. A passing build proves the code compiles; `/check` proves
the thing the spec promised actually happens. It changes no source and commits
nothing - it runs the app and reports what it saw.

## Input

Optional: a specific thing to check (a step, a flow, a URL). With no argument,
verify the whole current feature against every "done when" in
`blueprint/context/current-feature.md`.

## Step 1 - build the checklist

Read `blueprint/context/current-feature.md`. Pull the observable "done when"
criteria from the build steps (and any acceptance notes in the Testing section).
Turn them into a concrete checklist of claims to prove - each one a specific,
observable behavior, not "it works". If the user named one thing, scope to that.

If there's no current feature spec, ask what to verify rather than guessing.

## Step 2 - get the app running

Use the project's real commands (see Commands in `AGENTS.md`). Match the project
type:

- **Web app** - start (or reuse) the dev/preview server, then drive a real browser
  to the relevant routes. Prefer reusing an already-running server over starting a
  duplicate.
- **CLI** - run the actual command(s) with representative inputs.
- **Server/API** - start it and hit the endpoints.
- **Library** - exercise the public API through an example or the test command.

If a `test` command is declared in `AGENTS.md`, you may run it as *one* input, but
`/check` is broader than unit tests: it checks real behavior, which is exactly the
evidence UI and integration steps ride on instead of unit tests.

## Step 3 - exercise each claim

Drive the app to each checklist item and capture evidence as you go:

- Navigate and interact for real (click, type, submit, download) - don't assert
  from the code what the running app would do.
- Capture **screenshots** for visual/UI claims, **output** for CLI/API claims.
- Watch for **console errors and failed network requests**; a clean-looking screen
  with errors in the console is not a pass.

## Step 4 - report

Give a short, honest verdict, one line per checklist item:

    [pass] Download PDF saves certificate-<slug>.pdf - file downloaded, opened to the cert
    [pass] Both buttons show a loading state - screenshot: loading-state.png
    [fail] PDF border missing - printBackground not set; screenshot: pdf-no-border.png
    [skip] Vercel render - can't verify locally (feature 9)

Then state the bottom line: are all the feature's done-whens proven, or not yet.

- All proven -> say it's ready for `/complete`.
- Anything failed -> hand back to `/implement` to fix; name what to fix. Don't fix
  it here.
- Anything unverifiable -> say so plainly and why; never report it as a pass.

## Rules

- **Observe, don't change.** `/check` runs the app and reports. It never edits
  source, never commits, never merges. Fixing is `/implement`'s job.
- **Evidence or it didn't happen.** Every `pass` is backed by something observed -
  a screenshot, output, a response. No assumed passes from reading the code.
- **Honest over green.** "Couldn't verify" and "failed" are valid, useful results.
  Faking a pass defeats the entire gate.
- **Check the spec, not vibes.** Verify against the done-whens in
  `current-feature.md`, so "works" means what the spec said it would do.
