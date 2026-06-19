# AGENTS.md

Instructions for AI coding agents working in this project. This is the cross-tool
entry point: Codex, Cursor, GitHub Copilot, Gemini CLI, Aider, Zed, Windsurf, and
others read `AGENTS.md`. Claude Code reads `CLAUDE.md`, which imports this file, so
there is a single source of truth.

## What this is

A description of your project and the problem it solves.

This project is built with the **AI Coding Blueprint**, a workflow layer, not an
app skeleton. To start a new project, scaffold the app first in an empty folder
(create-next-app, Vite, etc.), then overlay these files on top. Never run a
framework scaffolder inside a directory that already holds the blueprint files
(`AGENTS.md`, `CLAUDE.md`, `context/`, `docs/`, `.claude/`); it fails because the
directory isn't empty.

New here? `README.md` explains the whole workflow.

## Read these for full context

- `context/project-overview.md` - the project's source of truth
- `context/coding-standards.md` - conventions to follow
- `context/ai-interaction.md` - how to work with the user on this project
- `context/current-feature.md` - the one feature or fix being built right now

## Workflow

Build one feature or fix at a time, behind review gates. Each step's instructions
are a plain markdown file any agent can read and follow:

- `.claude/skills/overview/SKILL.md` - distill the two planning docs into `context/project-overview.md`
- `.claude/skills/feature/SKILL.md` - turn a build-plan item into a spec in `context/current-feature.md`
- `.claude/skills/fix/SKILL.md` - document an ad-hoc bug or change into `context/current-feature.md`
- `.claude/skills/implement/SKILL.md` - build the current spec one small, reviewed step at a time
- `.claude/skills/complete/SKILL.md` - log it (to `docs/features/` or `docs/fixes/`) and merge
- `.claude/skills/prototype/SKILL.md` - optional, pre-build: static mockups to lock the look

In Claude Code these run as slash commands (`/overview`, `/feature`, and so on).
In other tools, perform the same step by following the matching `SKILL.md` when
the user asks for it (for example, "run the overview" means follow
`.claude/skills/overview/SKILL.md`). The conventions in `context/` apply however a
step is invoked, and the review gates are not optional: small steps, and the user
approves each diff before it lands.

## Commands

For a standard Next.js project. Change or remove if you're using something else.

- Dev server: `npm run dev` (http://localhost:3000)
- Build: `npm run build`
- Production server: `npm run start`
- Lint: `npm run lint`
- Test: `npm run test` (single run)
- Test watch: `npm run test:watch`
