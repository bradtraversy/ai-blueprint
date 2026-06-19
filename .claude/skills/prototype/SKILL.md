---
name: prototype
description: Interactively prototype the look of a project. Asks about the desired look and feel and which pages to draft, proposes a plan, and only then writes throwaway static HTML/CSS mockups to prototypes/ that share one theme (a set of CSS theme variables). A plan-first, pre-build helper that sits outside the spec-driven feature loop, like scaffolding. Use when the user runs /prototype, names screens to mock up, or asks to prototype the layout, theme, or look and feel.
---

# prototype - lock the look before you build

Where this sits in the workflow:

    plan  ->  /overview  ->  [prototype]  ->  /feature  ->  build
    (you      (project-      (lock the       (one spec    (real
     write)    overview.md)   look)           at a time)   code)

Prototyping is a pre-build step, not a feature. It's fast, visual, and throwaway.
Its one durable output is the theme: a set of CSS theme variables that port into
the real app's `globals.css` `@theme` when you build the first UI feature.
Everything else here gets discarded.

**This skill is plan-first.** Gather the look and the page list, agree on a plan,
and only then write any files. Never generate mockups before the user approves.

## Step 1 - read what the plan already says

Pull the stated look and feel and the screen/route list from
`context/project-overview.md` (its UI/UX section); fall back to the UI/UX section
of `docs/planning/project-plan.md` if the overview isn't generated yet. Use this
as the starting point, so you're refining the user's intent, not asking from
scratch.

## Step 2 - ask about the look and the pages

Work in plan mode. Ask the user a short set of questions (use AskUserQuestion for
the discrete choices), seeded with what the plan already says:

- **Look and feel** - confirm or adjust the vibe (light/dark, minimal/rich,
  density, editor-like, and so on), and ask for any reference apps or sites they
  want it to feel like.
- **Color and type** - any accent color or font direction (for example,
  mono-forward for code).
- **Which pages** - which screens to draft now. Default to the key routes from the
  plan; let the user add, drop, or reorder. Lean to a few, not every screen.

Keep it short. The user's answers, plus the plan, are the brief for the mockups.

## Step 3 - propose the plan, then wait

Present a short plan and stop for approval:

- the theme direction in a sentence or two (the vibe, accent, fonts), and
- the list of screens you'll mock, one line each on what each will show (the real
  states that exercise the theme, not empty shells).

Write nothing until the user approves. Adjust the plan if they push back.

## Step 4 - lock one theme

Once approved, write a single shared `prototypes/theme.css` that defines the theme
as CSS variables, following `reference/theme-variables.css`: surfaces, text,
accent, any component-specific states, font stacks, and a small scale. Derive the
values from the agreed brief.

This file is the deliverable. Keep it the single source of the theme, so tweaking
it restyles every mockup at once.

## Step 5 - mock each screen

For each approved screen, write a self-contained `prototypes/<screen>.html` that
links `theme.css` and lays out that screen with realistic dummy content and the
states that matter (a typing page mid-type with correct/wrong/pending chars and a
caret; a dashboard with believable stats and history rows; and so on).

- Plain HTML + CSS only. No framework, no build step. A few lines of inline JS for
  a view toggle is fine; nothing more.
- Pull every color, font, and spacing value from the `theme.css` variables, never
  hard-coded. That's what keeps the look consistent and portable.
- Realistic placeholder content over lorem ipsum. Desktop-first is enough.

## Then stop

Tell the user to open the files in a browser and iterate on the look. When the
theme feels right, the variables in `theme.css` carry into `globals.css` `@theme`
at the first UI feature; the HTML mockups are reference and get discarded. This
skill locks the look, it does not build the app.
