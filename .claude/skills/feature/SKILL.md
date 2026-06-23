---
name: feature
description: Turn a feature from build-plan.md into a buildable spec. With no argument, specs the next unchecked item in the build plan; given a number or name, specs that one. Sizes the feature and splits anything too big into smaller sub-features (4a, 4b, ...), writes small, reviewable build steps to blueprint/context/current-feature.md, then red-teams its own draft for gaps, oversized steps, and scope creep before stopping at a review gate. Use when the user runs /feature, names or numbers a feature, or asks to spec out, break down, or start the next feature.
---

# feature - turn a build-plan feature into a buildable spec

Where this sits in the workflow:

    project-overview.md  +  build-plan.md  ->  [this skill]  ->  build
    (source of truth,        (which feature       (the spec for      (code,
     from /overview)          to build)            one feature)        reviewed)

`build-plan.md` is intentionally high-level - one line per feature, no detail,
no ordering ceremony. All of that is this skill's job: take one listed feature,
read the full context from `project-overview.md`, and turn it into something
buildable.

## Input

A feature from `build-plan.md`, by number or name - e.g. `/feature 3` or
`/feature "typing engine"`.

**With no argument, build the next one.** `/feature` on its own specs the first
unchecked item in `build-plan.md`. The build plan is a checklist; finished
features are checked off, so the first unchecked item is always what's next. (If a
big item has been split into sub-items, the next unchecked sub-item is the target.)

## Step 1 - pick the target

- Given a number or name -> use it.
- No argument -> read `build-plan.md` top to bottom and take the first unchecked
  leaf (a plain item, or a sub-item under one that was split).

**If the build plan isn't a checklist yet** - a plain list with no `- [ ]` boxes -
treat every item as unchecked: take the first item as the target, and offer to
convert the list to a checklist so progress is trackable from here on. Proceed
with the first item whether or not the user wants the conversion.

State which feature you're building before going further.

## Step 2 - size it, and split if too big

Read the target line from `build-plan.md`, then pull full context from
`blueprint/context/project-overview.md` (the data model, stack, and conventions). Decide
how big the feature is:

- **Small enough to build and review as one unit** -> one spec. Continue to
  Step 3.
- **Too big for one reviewable spec** -> split it. Propose a short list of
  sub-features in chat (title + one line each), let the user adjust it, then write
  those sub-items back under the parent in `build-plan.md` as an indented
  checklist (`4a`, `4b`, `4c` ...). Spec only the **first** sub-feature now; the
  rest get picked up on later `/feature` runs.

Two levels of breakdown - don't confuse them:

- **Sub-features** (here) - each is big enough to stand alone: its own branch,
  spec, review-and-merge cycle, and archive entry.
- **Build steps** (in the spec, Step 3) - small diffs *within* one feature.

Worked example - "Authentication" is too big for one spec, so it splits into
sub-features in `build-plan.md`:

    - [ ] 4. Authentication
      - [ ] 4a. Registration - sign-up page + create Profile and handle
      - [ ] 4b. Login - sign-in page + session
      - [ ] 4c. Route protection - gate saving/drills/leaderboard, plus sign-out

Then *within* 4a, the build steps are small: first "registration page UI", then
"register server action + validation + redirect". The page and its logic are
steps, not separate features.

This sizing call is the skill's job, not the build plan's - that's exactly why the
build plan starts high-level.

## Step 3 - write the spec

For the one (sub-)feature being built now, write a full spec to
`blueprint/context/current-feature.md` (create `blueprint/context/` if needed), following
`reference/feature-spec-template.md`. Fill every section: goal, in/out of scope,
the build loop, small build steps as a checklist (`- [ ]`, each with an observable
"done when" - `/implement` ticks them off and resumes from the first unchecked
one), files/areas, data/contracts, testing, and notes for the AI.

**Visual or replication features need a reference image.** If the feature is
"make it look like X" - recreating an existing design, matching a mockup, or
rebuilding a Canva/Figma artifact - prose underspecifies the target and the build
will approximate it wrong. Ask the user for a screenshot or image if one isn't
already provided, save it under `blueprint/reference/` (create the folder if
needed), and link it from the spec's Design reference section. Don't write a
visual spec from words alone when an image could exist.

This is a draft. Don't present it yet - critique it first.

## Step 4 - red-team the draft, then tighten

Before the user reads it, turn on the spec yourself and try to break it. The
cheapest place to catch a scope problem or an oversized step is here, before any
code exists. Run the draft against these questions:

- **Coverage.** What does this feature need that no step delivers? Push on the
  unhappy paths the happy-path spec skipped: empty / missing / malformed input,
  the error / loading / empty states, the first-run case, failure of anything
  external it calls.
- **Visual fidelity.** If this is a look-alike or replication feature, is a
  reference image linked in the spec - or are we about to build a design blind
  from prose? If a real design exists and no image is captured, get one before
  building, not after the approximation lands.
- **Step size.** Would any step's diff be too big to read in one sitting? If so,
  split it - oversized steps defeat the review gate.
- **Order.** Does each step leave the app working, and depend only on earlier
  steps, never a later one? Resequence if not.
- **Contracts.** Is any type, route, or stored shape that a later feature will
  touch left undefined here? Lock it now and flag it load-bearing.
- **Scope honesty.** Is anything creeping in that belongs to a later feature? Is
  anything pushed to "out of scope" that this feature actually can't ship without?
- **Done-whens.** Is each one observable and checkable by `/check`, or is it a
  vague "it works"? Make it concrete.
- **Testing.** Does the predicted coverage match the gate - in-scope logic gets a
  test when a `test` command is declared in `AGENTS.md`, UI/integration rides on
  screenshot + build?

Apply the fixes to `current-feature.md`. Then stop and present the spec, leading
with a short **"what the critique changed"** note - the splits, gaps, or scope
cuts you made (or "nothing - the draft held up"). That note is the point: it shows
the gate working before a line of code is written.

Tell the user to review and adjust. This skill plans; it never starts building.

## Rules the spec must follow

- **Small, reviewable steps.** Each step ends with something working and a diff
  small enough to read in full. If a step's diff would be too big to review, the
  step is too big - split it. This review gate is the point.
- **Build in order.** Sequence the steps so each builds on the last and leaves
  the app working.
- **Lock data contracts early.** If a shape (type, API response, stored field) is
  used by a later feature, define it now and flag it as load-bearing.
- **Flag client vs server** and any conventions from `blueprint/context/coding-standards.md`
  (for example, filtering user-scoped queries by the authenticated user's id).
- **Scope honestly.** State what is deferred so the feature stays contained.

## When a (sub-)feature is done

Check its box in `build-plan.md` (and the parent item once all its sub-items are
checked), archive the finished `blueprint/context/current-feature.md` to
`blueprint/history/features/NN-name.md`, then run `/feature` again for the next one.
