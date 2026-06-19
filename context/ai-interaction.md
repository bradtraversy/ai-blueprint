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

Run `/feature` to write the spec, `/implement` to build it on a branch, and
`/complete` to log it and merge. The numbered loop below is what those skills
follow (and what you do by hand for small fixes).

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
6. **Iterate** - Adjust if needed.
7. **Commit** - Only after the build passes and everything works.
8. **Merge** - Merge to main.
9. **Delete branch** - After merge.
10. **Complete** - Archive the spec to `docs/features/NN-name.md`, check the
    feature off in `docs/planning/build-plan.md`, and reset
    `context/current-feature.md` to its stub.

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
