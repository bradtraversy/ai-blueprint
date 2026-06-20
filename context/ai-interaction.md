# AI Interaction Guidelines

> **This blueprint is an overlay layer**, added on top of an already-scaffolded
> app. Never run a framework scaffolder (create-next-app, etc.) inside this
> directory. For a new project, scaffold the app first, then overlay these files.

## Communication

- Be concise and direct
- Explain non-obvious decisions briefly
- Ask before large refactors or architectural changes
- Don't add features not in the project spec
- Never delete files without clarification

## Workflow

The loop we use for every feature. The spec for the feature being built lives in
@context/current-feature.md.

Run `/feature` (or `/fix` for a bug or change that isn't a planned feature) to
write the spec, `/implement` to build it on a branch, and `/complete` to log it
and merge. The numbered loop below is what those skills follow.

The skills are the structured path, not a requirement. You can also just describe
a feature, fix, or change in chat at any time and we'll build it the same way; the
rules below still apply (small steps, you approve each diff, the conventions in
`coding-standards.md`), because they're always in context. Use the skills when you
want the repeatable loop and the logging; prompt directly when you just want
something done.

1. **Spec** - Run `/feature` (no number = the next unchecked item in
   `build-plan.md`) to generate @context/current-feature.md, then review it
   together before any code.
2. **Branch** - Create a new branch for the feature/fix.
3. **Implement** - Build one small step from the spec at a time, not the whole
   feature at once.
4. **Review** - Show the diff (not full files). I read and approve each step
   before moving to the next.
5. **Test** - Verify in the browser. Run `npm run test` for unit tests and
   `npm run build` to check for errors.
6. **Iterate** - If it doesn't work or needs changes, re-prompt or hand-edit and
   re-test; repeat until it works, before moving on.
7. **Checkpoint (optional)** - after an approved step `/implement` pops a quick
   choice (commit a checkpoint / continue / stop here). Checkpoints are optional
   cheap rollback points; `/complete` makes the real feature-level commit. Build
   and tests must pass before any commit.
8. **Log** - `/complete` archives the spec to `docs/features/NN-name.md` (or
   `docs/fixes/`), checks the feature off in `docs/planning/build-plan.md`, and
   resets `context/current-feature.md` to its stub.
9. **Feature commit** - `/complete` stages everything on the branch (step work
   plus the logging changes) into one conventional feature commit.
10. **Squash-merge** - `/complete` squash-merges the branch to main (explicit yes)
    and deletes it, so the feature lands as one commit; push stays a separate
    explicit yes.

Do NOT commit without permission or until the build and tests pass. If build or
tests fail, fix the issues first.

## Branching

A new branch for every feature/fix. Name it **feature/[name]** or
**fix/[name]**. Ask to delete the branch once merged.

## Commits

- Ask before committing (don't auto-commit)
- Use conventional commit messages (feat:, fix:, chore:, etc.)
- Keep commits focused (one feature/fix per commit)
- Never put "Generated with Claude" or any AI attribution in commit messages

## When Stuck

- If something isn't working after 2-3 attempts, stop and explain the issue
- Don't keep trying random fixes
- Ask for clarification if requirements are unclear

## Code Changes

- Make minimal changes to accomplish the task
- Don't refactor unrelated code unless asked
- Don't add "nice to have" features
- Preserve existing patterns in the codebase

## Code Review

Review AI-generated code periodically, especially for:

- Security (auth checks, input validation)
- Performance (unnecessary re-renders, N+1 queries)
- Logic errors (edge cases)
- Patterns (matches existing codebase?)
