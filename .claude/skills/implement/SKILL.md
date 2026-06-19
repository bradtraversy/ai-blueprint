---
name: implement
description: Build the feature or fix spec'd in context/current-feature.md, one small reviewable step at a time. Creates the branch, implements each step, shows the diff and explains it in plain English, tests, and iterates on feedback until it works before committing. Builds and commits on the branch only; hand off to /complete to merge and log. Use when the user runs /implement, or asks to build, implement, or start the current feature or fix once its spec is ready.
---

# implement - build the current feature or fix, one reviewed step at a time

Where this sits in the workflow:

    /feature or /fix  ->  [implement]  ->  /complete  ->  next
    (the spec)            (build it,       (log it +
                           reviewed)        merge)

`/feature` (or `/fix`) wrote the spec to `context/current-feature.md` and stopped.
This skill turns that spec into code, following the build loop in
`context/ai-interaction.md`, without vibe coding: small steps, a visible diff plus
a plain-English explanation for each, testing, and iteration until it works, all
behind your approval. It builds and commits on a branch; merging and logging are
`/complete`'s job.

## Before you start

Read `context/current-feature.md`. If it has no real spec (still the stub, or its
status is already complete), stop and tell the user to run `/feature` (for a
planned feature) or `/fix` (for an ad-hoc bug or change) first. Pull the
conventions from `context/coding-standards.md` and the data model from
`context/project-overview.md` so the code matches them.

## Step 1 - branch

Create and check out a branch named from the spec: `feature/<name>` for a feature,
`fix/<name>` for a fix. If the project isn't a git repo yet, say so and ask the
user to run `git init` first; the loop needs branches.

## Step 2 - build one step, review, iterate, commit

Work through the spec's build steps in order, one at a time. For each step:

1. Implement just that step: the smallest change that satisfies its "done when."
2. Show the **diff**, not whole files.
3. **Explain it.** For each file created or changed, one or two sentences on what
   it does and any key function or pattern. This is the anti-vibe-coding gate; the
   user should understand every change.
4. **Test it.** Run the project's build and tests if it has them (for example
   `npm run build`, `npm run test`) and confirm the step's "done when" is met.
5. **Iterate until it works.** If it fails or the user wants changes, revise the
   step (re-prompt or hand-edit the code), show the updated diff, and re-test.
   Repeat until it works and the user approves. Nothing is committed until the
   user is happy with the step.
6. **Commit.** Once approved, commit that step with a conventional message (ask
   before committing), then move to the next step.

Never batch the whole thing into one diff. If a step's diff is too big to read,
split it. Build and tests must pass before a commit.

## Step 3 - hand off to /complete

When every step is built, committed, and the build and tests pass, stop. Tell the
user it's done on its branch and to run `/complete` to log it (archive, check off
if it's a feature, reset) and merge. This skill does not touch main.

## Rules

- One small step per diff; the user reviews and approves each before any commit.
- Explain every change in plain English. Understanding the code is the point.
- Iterate on the branch until each step works; never commit code the user hasn't
  approved.
- Follow `context/coding-standards.md` (server vs client, scope user-owned queries
  by the authenticated user id, validate inputs, and so on).
- Build only what the spec says. If the spec is wrong or thin, stop and fix the
  spec first, do not improvise.
- Build and commit on the branch only. Don't merge or push here; that's
  `/complete`.
