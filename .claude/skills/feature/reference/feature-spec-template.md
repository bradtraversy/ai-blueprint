# Feature: <name>

**From build-plan:** feature <n>
**Status:** not started

## Goal

What this feature delivers, in a sentence or two. Why it matters.

## In scope

- The specific things this feature includes.

## Out of scope

- What it deliberately doesn't touch (deferred to a later feature).

## Build loop

Build one step at a time, never the whole feature at once.

1. Plan mode lays out the step before any code.
2. The AI implements just that step.
3. It shows the diff (not full files); you read it and understand it.
4. You approve and commit, then move to the next step.

Never accept a step you haven't read. If a diff is too big to review, the step was too big, so split it.

## Build steps

Small, reviewable units. Each ends with something working.

1. **<step>** - what you build. *Done when:* <observable criteria>.
2. **<step>** - what you build. *Done when:* <observable criteria>.

## Files / areas

- The files or modules this will create or change.

## Data / contracts

- Schema, types, or API shapes involved, or "none yet."

## Testing

- How to verify: what to unit-test, what to click through.

## Notes for the AI

- Conventions and constraints to respect (e.g. client vs server, filter user-scoped queries by the authenticated user's id, match an existing data shape).
