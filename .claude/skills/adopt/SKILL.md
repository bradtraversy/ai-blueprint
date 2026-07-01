---
name: adopt
description: Bring the blueprint into an existing (brownfield) codebase. Surveys the real repo, interviews you for the intent code can't reveal, then generates the two plans you own (project-plan.md with shipped features, build-plan.md as a checklist with existing features already checked) plus coding-standards.md reflecting the project's actual conventions - and points you at /overview to finish. Use when the user runs /adopt, is overlaying the blueprint onto an app that already has meaningful code, or asks to adopt or bootstrap the workflow into an existing project. For freshly scaffolded or early projects, use onboard instead.
---

# adopt - bootstrap the blueprint from an existing codebase

Where this sits in the workflow:

    existing codebase  ->  [adopt]  ->  project-plan + build-plan + coding-standards  ->  /overview  ->  normal loop
    (already has code)     (survey +     (seeded from the real code; shipped               (project-       (/feature,
                            interview)     features already checked off)                    overview.md)     /implement, ...)

The standard onboarding assumes a freshly scaffolded, near-empty app: you write
the two plans from scratch and build forward. That doesn't fit a project that
already has thousands of lines of working code. `/adopt` is the brownfield
on-ramp: it reads what's already there, asks you only for what the code can't tell
it (the *why* and the *roadmap*), and produces the same input files the rest of
the workflow expects - so an existing project joins the loop without you
hand-writing everything.

It generates the inputs; it does not generate `project-overview.md`. That stays
`/overview`'s job. `/adopt` ends by telling you to run `/overview`.

## Input

A description of what the project is, if the user offers one. Otherwise just the
repository itself. No argument is required.

## Step 0 - confirm it's brownfield and safe

Look at `blueprint/project-plan.md` and `blueprint/build-plan.md`.

- If they're missing or still the empty worksheet/placeholder, proceed.
- If they already hold real content, this project is already adopted. Stop and say
  so; offer to refresh a specific file instead of overwriting work the user owns.

Never overwrite a filled-in plan without explicit confirmation. Never run a
framework scaffolder (the blueprint is an overlay, never a generator).

## Step 1 - survey the codebase (read-only)

Read the repo to establish the facts. Change nothing in this step. Establish:

- **Stack and tooling** - language(s), framework(s), and versions, from the real
  manifest (`package.json`, `requirements.txt`, `pyproject.toml`, `go.mod`,
  `Gemfile`, `Cargo.toml`, etc.). Note the package manager actually in use (lockfile).
- **Commands** - the real dev / build / test / lint scripts. These feed the
  Commands section of `AGENTS.md` and, per the testing opt-in switch, decide
  whether a testing gate even applies.
- **Conventions in practice** - directory layout, component/file naming, styling
  approach, state management, data-fetching pattern, error handling. Read what the
  code *does*, not what a default template prescribes.
- **Testing reality** - is a runner configured and are there tests, or none? Be
  honest; don't describe a gate the project doesn't have.
- **What the app already does** - the shipped features, inferred from routes,
  pages, entry points, and modules. This becomes the *checked* part of the build plan.

Keep notes; you'll turn them into the files in Step 3.

## Step 2 - interview for intent

The code reveals *what* and *how*, never *why* or *what next*. Ask the user a short
set of questions (aim for three to five, not an interrogation) to fill the gaps:

- What is this project for, and who uses it? (the problem and the users)
- Is the stack and structure you found intentional, or are there parts they'd call
  legacy / want to change?
- What do you want to build next? (the unchecked items in the build plan)
- Anything the survey got wrong or missed?

If the user already gave intent up front, skip what they've answered. Don't ask
what you can read from the code.

## Step 3 - generate the inputs

Write these, drawn from the survey (facts) and the interview (intent). Mark every
inference you're unsure of with a clear `> TODO (confirm)` so the user can correct
it rather than inherit a wrong guess.

- **`blueprint/project-plan.md`** - the what & why, following the existing
  worksheet structure (problem, users, features, data, tech, monetization, UI/UX).
  The "features" and "tech" sections describe what *already exists*; the rest comes
  from the interview.
- **`blueprint/build-plan.md`** - the ordered feature list as a checklist. **Mark
  shipped features `- [x]`** (this is the brownfield difference: the build plan
  reflects reality, so most of an existing app starts checked) and the roadmap
  items from the interview as `- [ ]`. This makes `/status` and `/feature` work
  immediately - the next unchecked item is genuinely what's next.
- **`blueprint/context/coding-standards.md`** - rewrite the default to match the
  project's *actual* conventions from Step 1, not the shipped Next.js/Prisma
  defaults. Keep the Writing and Comments sections; replace the stack-specific ones
  with what the code really does. Its Testing section must reflect the real testing
  state (the opt-in switch is a `test` command in `AGENTS.md`).
- **`AGENTS.md` Commands section** - fill in the real dev / build / test / lint
  commands you found, so the rest of the workflow (and the testing gate) uses the
  project's actual scripts.

Do not write `project-overview.md`; that's `/overview`'s job, downstream of these.

## Step 4 - review gate, then hand off

Stop and show the user what you generated, calling out:

- the **build-plan split** - what you marked shipped vs not, since that's the
  judgment most worth their eyes,
- every `> TODO (confirm)` you left,
- anything the survey and the interview disagreed on.

These files are the ones the user *owns*. Have them review and adjust, then tell
them to run `/overview` to distill the plans into `project-overview.md` and start
the normal loop.

## Rules

- **Read-only until Step 3.** The survey changes nothing; only generation writes.
- **Reflect reality, don't prescribe.** `coding-standards.md` must match the code
  that exists. A project using Zustand and REST routes should not be handed
  standards about Server Actions and Prisma just because that's the default.
- **Never invent intent.** Ask for the why and the roadmap; mark anything inferred
  with `> TODO (confirm)`. Silent guesses about purpose are the main failure mode.
- **Don't clobber owned work.** If the plans already have real content, confirm
  before touching them. Never run a scaffolder.
- **Be honest about testing.** If there's no runner, say testing is opt-in and not
  yet set up; don't describe a gate the project hasn't adopted.
