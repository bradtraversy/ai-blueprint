---
name: complete
description: Wrap up a finished feature. Archives its spec to docs/features/, checks it off the build plan, resets context/current-feature.md to its stub, then merges the feature branch to main and deletes it. Merges only with explicit approval and never pushes without a "yes". Use when the user runs /complete, or asks to finish, wrap up, merge, or close out the current feature after it's built and reviewed.
---

# complete - log the finished feature and merge

Where this sits in the workflow:

    /feature  ->  /implement  ->  [complete]  ->  next feature
    (the spec)    (build it)      (log + merge)   (/feature again)

`/implement` built the feature on its branch and committed each reviewed step.
This skill closes it out: it logs the feature, resets the working file, and merges.
Run it only when the build is done, reviewed, and the build and tests pass.

## Before you start

Confirm the feature is actually finished: `context/current-feature.md` holds a real
spec, its steps are built and committed on a feature branch, and the build and
tests pass. If work is unfinished or uncommitted, stop and say so.

## Step 1 - log the feature

1. Archive `context/current-feature.md` to `docs/features/NN-name.md` (NN is the
   build-plan number, the name from the feature). This file is the build history.
2. Check the feature off in `docs/planning/build-plan.md` (and its parent item if
   this was a split sub-feature, once all its sub-items are checked).
3. Reset `context/current-feature.md` to its stub ("no feature in progress").
4. Commit these doc changes on the feature branch.

## Step 2 - merge

1. Merge the feature branch into main, only with the user's explicit go-ahead.
2. Delete the feature branch after a clean merge.
3. Never push without the user explicitly saying so. If they do, push main once.

Then point the user at `/feature` for the next item.

## Rules

- Don't merge unfinished or failing work; the build and tests must pass first.
- Merging and pushing are the user's calls: get an explicit yes for the merge, and
  a separate explicit yes before any push.
- One feature per completion. If a parent item still has unchecked sub-features,
  leave the parent unchecked.
