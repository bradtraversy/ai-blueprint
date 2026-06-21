---
name: overview
description: Generate blueprint/context/project-overview.md from the two planning docs (blueprint/project-plan.md and blueprint/build-plan.md). The overview is the single AI-facing source of truth that CLAUDE.md loads every session. Use when the user runs /overview, has just finished writing or editing the plans, or asks to (re)generate the project overview.
---

# overview - turn the two plans into the AI-facing source of truth

Where this sits in the workflow:

    project-plan.md  +  build-plan.md  ->  [this skill]  ->  project-overview.md  ->  /feature  ->  build
    (what & why,         (high-level                          (the one doc the         (one spec
     written by you)      feature list,                        AI reads every           at a time)
                          written by you)                      session)

You provide two files: `blueprint/project-plan.md` (what & why) and
`blueprint/build-plan.md` (the ordered feature list), drafted by you or with
the AI's help; what matters is that you own their content. Everything else in the
workflow is generated from those two. This skill is the first generation step: it
distills both plans into `blueprint/context/project-overview.md`, the single doc CLAUDE.md
loads at the start of every session.

## Input

The two planning docs, already written:

- `blueprint/project-plan.md` - problem, users, features, data, tech,
  monetization, UI/UX
- `blueprint/build-plan.md` - the ordered, one-line-per-feature build list

If either is missing or still has placeholder text, stop and tell the user to
fill it in first. This skill distills plans; it does not invent them.

## Step 1 - read both plans

Read `project-plan.md` and `build-plan.md` in full. Note where they disagree - a
feature in the build plan the project plan never mentions, a data point no
feature uses, a stack choice that contradicts a standard. You will surface
these, not paper over them.

## Step 2 - synthesize the overview

Write `blueprint/context/project-overview.md` (create `blueprint/context/` if needed), following
`reference/project-overview-template.md`. The overview is a consolidation, not a
copy:

- **One source of truth.** Merge both plans into one coherent document. After
  this runs, the AI reads the overview, not the raw plans.
- **Make the data model concrete.** Turn the plan's data list into actual
  models with fields, types, and relationships, derived from the features that
  use them. This is the most valuable thing the overview adds.
- **Tie features to build order.** List the features with a one-line purpose
  each, in build-plan order, so the AI knows what exists and what's next.
- **Stay faithful.** Don't add features, data, or stack choices that aren't in
  the plans. If something is underspecified, leave a clearly marked `> TODO`
  rather than inventing an answer.

Then stop. Report what you wrote and list any contradictions or gaps you found
between the two plans, so the user can fix the plans and re-run.

## Rules

- **Generated, not authored.** Treat `project-overview.md` as a build artifact of
  the two plans. When the plans change, re-run this skill rather than hand-editing
  the overview.
- **No new scope.** Everything in the overview must trace back to one of the two
  plans. Invented scope is the main failure mode here.
- **Concrete over vague.** Field-level data models and named routes beat
  restating the plan's one-liners.
- **Surface conflicts.** Always end by reporting disagreements between the plans;
  silent reconciliation hides decisions the user should make.

## When to re-run

Re-run whenever `project-plan.md` or `build-plan.md` changes materially - a new
feature, a changed data model, a different stack. The overview is downstream of
the plans and should be regenerated, not patched.
