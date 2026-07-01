---
name: doctor
description: "Run a read-only Blueprint health check for setup, onboarding, required files, tool adapters, commands, ignore rules, planning readiness, overview freshness, and workflow drift. Use when the user runs /doctor, asks whether the Blueprint is installed correctly, wants a health check, setup check, doctor pass, or says something feels off before starting or resuming work."
---

# doctor - Blueprint health check

Where this sits in the workflow:

    any time  ->  [doctor]  ->  reads setup + plans + workflow state + git
                  (read-only)   prints health, warnings, and repair order

This skill answers one question: *is this Blueprint project ready to use?* It is
the diagnostic pass for setup drift, incomplete onboarding, missing files,
placeholder plans, stale generated context, and confusing workflow state. It
never changes anything: no edits, no commits, no installs, no builds, no branch
changes.

Use `/status` when the user mainly wants progress and the next build action. Use
`/doctor` when the user wants to know whether the workflow itself is healthy.

## Input

None. `/doctor` takes no argument.

## What it checks

Gather these, then summarize. Do not dump file contents.

1. **Required Blueprint files**
   - Confirm `AGENTS.md`, `blueprint/`, `blueprint/project-plan.md`,
     `blueprint/build-plan.md`, and `blueprint/context/` exist.
   - Confirm `blueprint/context/coding-standards.md`,
     `blueprint/context/ai-interaction.md`,
     `blueprint/context/current-feature.md`, and
     `blueprint/context/project-overview.md` exist.
   - Confirm `blueprint/history/features/` and `blueprint/history/fixes/` exist.
2. **Tool adapters**
   - Confirm at least one adapter exists: `.agents/skills/` for Codex or
     `.claude/skills/` for Claude Code.
   - If both adapters are present, say that is healthy when both tools are used.
   - If only one tool is used, mention the unused adapter can be deleted. Do not
     treat extra adapters as an error.
   - If `CLAUDE.md` exists and still starts with `# Project Name`, flag that
     `/onboard` probably has not finished.
3. **Commands and project setup**
   - Check whether `AGENTS.md` has a `## Commands` section with dev and build
     commands.
   - Report missing lint or test commands as informational unless the project has
     real lint or test scripts elsewhere that are not reflected in `AGENTS.md`.
   - If `package.json` exists, compare its scripts against `AGENTS.md` at a high
     level. Do not require every script to be documented.
4. **Ignore rules**
   - Check obvious ignore patterns for the detected stack. For Node or Astro,
     look for `node_modules`, `.env`, `dist`, and framework cache folders such as
     `.astro` or `.next` when relevant.
   - Keep this conservative. If uncertain, report "review" instead of failure.
5. **Planning readiness**
   - Check whether `blueprint/project-plan.md` and `blueprint/build-plan.md` look
     filled in or still template-like. Treat obvious TODO, TBD, example-only text,
     or empty required sections as not ready.
   - Count checked and unchecked leaf items in `blueprint/build-plan.md`.
6. **Overview freshness**
   - Check whether `blueprint/context/project-overview.md` exists and looks
     generated from the current plans.
   - If either planning file appears newer than the overview by filesystem time,
     call the overview possibly stale and suggest `/overview` before feature work.
7. **Current workflow state**
   - Check whether `blueprint/context/current-feature.md` is the reset stub or an
     active feature or fix spec.
   - If a spec is active, report checked and unchecked implementation steps.
   - Flag active spec on `main`, all spec steps checked but no completion, or a
     mismatch between the active spec and the next unchecked build-plan item.
8. **Git**
   - Report current branch, clean vs dirty working tree, rough changed-file count,
     last commit subject, and whether the branch is ahead of upstream.
   - If the directory is not a git repo, report that as a setup issue and keep
     going.

## Output

Print a compact health report with these labels:

    Health: Pass | Needs attention | Blocked
    Setup: ...
    Adapters: ...
    Plans: ...
    Workflow: ...
    Git: ...
    Watch: ...
    Repair order: ...

Use `Watch:` only when there are warnings. Use `Repair order:` for the exact next
steps, in order. Keep it short and practical.

Choose the repair order in this priority:

- Required Blueprint files missing -> overlay the Blueprint again, or use
  `/adopt` for a brownfield app.
- No git repo -> initialize git before using the build loop.
- No tool adapter -> restore `.agents/skills/` or `.claude/skills/` for the tool
  being used.
- Onboarding incomplete -> run `/onboard`.
- Commands or ignore rules need review -> update the files or run `/onboard` if
  this is an early project.
- Plans are placeholders -> fill `blueprint/project-plan.md` and
  `blueprint/build-plan.md`.
- Overview missing or stale -> run `/overview`.
- Active spec has unchecked steps -> run `/status` or `/implement`, depending on
  whether the user wants orientation or action.
- Active spec is done but not closed -> run `/check`, then `/complete`.
- Everything is healthy -> say so, then suggest `/status` for progress or
  `/feature` for the next planned feature.

## Rules

- **Read-only, always.** This skill never writes files, never commits, never runs
  installs, never runs builds or tests, and never switches branches.
- **Diagnose, then order repairs.** Do not just list problems. End with the
  smallest ordered sequence that gets the project back to a healthy state.
- **Do not over-police adapters.** Extra adapters are optional clutter, not a
  failure.
- **Be conservative with stack-specific checks.** If a command or ignore pattern
  is uncertain, mark it for review instead of inventing a hard failure.
- **Stay concise.** A doctor pass should feel like a checklist, not an audit.
