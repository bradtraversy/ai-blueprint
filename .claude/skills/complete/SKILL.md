---
name: complete
description: Wrap up a finished feature or fix. Archives its spec to docs/features/ (feature) or docs/fixes/ (fix), checks features off the build plan, resets context/current-feature.md to its stub, then merges the branch to main and deletes it. Merges only with explicit approval and never pushes without a "yes". Use when the user runs /complete, or asks to finish, wrap up, merge, or close out the current feature or fix after it's built and reviewed.
---

# complete - log the finished work and merge

Where this sits in the workflow:

    /feature or /fix  ->  /implement  ->  [complete]  ->  next
    (the spec)            (build it)      (log + merge)

`/implement` built the feature or fix on its branch and committed each reviewed
step. This skill closes it out: it logs the work, resets the working file, and
merges. Run it only when the build is done, reviewed, and the build and tests pass.

## Before you start

Confirm the work is actually finished: `context/current-feature.md` holds a real
spec, its steps are built and committed on a branch, and the build and tests pass.
If work is unfinished or uncommitted, stop and say so.

## Step 1 - log the work

Check whether the spec is a feature or a fix (a fix is marked `Type: Fix` and has
no build-plan number).

- **Feature** - archive `context/current-feature.md` to `docs/features/NN-name.md`
  (NN is the build-plan number), and check it off in `docs/planning/build-plan.md`
  (and its parent item once all sub-items are checked).
- **Fix** - archive it to `docs/fixes/name.md`. A fix isn't a build-plan item, so
  there's nothing to check off.

Then reset `context/current-feature.md` to its stub ("nothing in progress") and
commit these doc changes on the branch. The archive is the build history.

## Step 2 - merge

1. Merge the branch into main, only with the user's explicit go-ahead.
2. Delete the branch after a clean merge.
3. Never push without the user explicitly saying so. If they do, push main once.

Then point the user at `/feature` (or `/fix`) for the next thing.

## Rules

- Don't merge unfinished or failing work; the build and tests must pass first.
- Merging and pushing are the user's calls: get an explicit yes for the merge, and
  a separate explicit yes before any push.
- One item per completion. If a parent feature still has unchecked sub-features,
  leave the parent unchecked.
