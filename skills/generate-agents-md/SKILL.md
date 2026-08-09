---
name: generate-agents-md
description: Write or reorganize a project-root AGENTS.md file that instructs coding agents about the repo. Use when the user asks to create, rewrite, or restructure AGENTS.md, or wants a canonical AGENTS.md template. Produces a flat structure (intro paragraph, Overview, Tech Stack, File Structure, Commands, Testing, Boundaries) with a README/AGENTS/STYLE separation of concerns.
---

# Skill: Write a project-root AGENTS.md

## What this skill produces

A project-root `AGENTS.md` that gives coding agents the operational knowledge they need to work in the repo safely: what the project is, how it is built and tested, and what they must never touch.

This skill targets the **project-root `AGENTS.md`** consumed by coding agents (Claude Code, OpenCode, Cursor, etc.).

## Separation of concerns

One document per audience. Do not duplicate.

| Document                       | Audience                         | Owns                                                                   |
| ------------------------------ | -------------------------------- | ---------------------------------------------------------------------- |
| `README.md`                    | Humans (contributors, users)     | Project overview, screenshots, dev quick-start, badges                 |
| `CONTRIBUTING.md` (if present) | Humans (contributors)            | Contribution workflow, PR process, code review expectations, branching |
| `DEPLOYMENT.md` (if present)   | Humans (operators, contributors) | Deploy steps, environment provisioning, release process                |
| `AGENTS.md`                    | Coding agents                    | Project knowledge, commands, testing, repo-level boundaries            |
| `STYLE.md` (if present)        | Both humans and agents           | Code style, naming, placement, API contracts, patterns                 |

Rules:

- `AGENTS.md` owns **environment, commands, testing, and repo-level boundaries** — not style, not human workflow.
- Style (naming, conventions, coding practices) lives in `STYLE.md`. `AGENTS.md` points to it with a single handoff line at the top.
- Human workflow (how to contribute, how to deploy) lives in `CONTRIBUTING.md` / `DEPLOYMENT.md`. `AGENTS.md` does not duplicate it — agents do not need contribution etiquette or deploy runbooks to edit code.
- If a convention is both human- and agent-relevant (e.g. naming), it belongs in `STYLE.md`, and `AGENTS.md` only references it.
- **No duplication.** Every fact lives in exactly one document. If `README.md` already states the run command, `AGENTS.md` may reference it but must not restate it — unless the agent needs an operational note the human doc omits (e.g. "never invoke `pip` directly — use `uv run`"). When in doubt, point, do not paste.

## Canonical structure

Exactly this order. Flat top-level sections — no nesting project knowledge under a parent `## Project` heading. Canonical sources (agents.md spec, Anthropic CLAUDE.md guidance) favour flat sections for agent scannability.

```
# AGENTS.md

<intro paragraph — no heading>

## Overview
## Tech Stack
## File Structure

## Commands

## Testing

## Boundaries
### ✅ Always
### ⚠️ Ask First
### 🚫 Never
```

### 1. Intro paragraph (no heading)

Two or three sentences directly under the `# AGENTS.md` title. No `## Introduction` heading. Cover:

- One-sentence project description (what it does, not why).
- One-sentence handoff to `STYLE.md` for style, and what THIS file owns instead.

Do not front-load philosophy, contributor thanks, or version history. The agent reads this first — make it dense.

### 2. ## Overview

Entry point, how to run, where the scheduler/process lives. 3–5 bullets.

### 3. ## Tech Stack

Language + version, package manager, database, frontend tooling, timezone/container quirks. One bullet per tool, with the version and the one-line "how this repo uses it" note. Include any intentional non-default choice (e.g. `pip` in Dockerfile instead of `uv`) so agents do not "fix" it.

### 4. ## File Structure

Flat list of `dir/` → one-line purpose. Only significant directories. Not a tree, not exhaustive.

### 5. ## Commands

A table with three columns: **Scope**, **Auto-fix**, **Verify**. Rows are scopes (Backend, Frontend, Migrations, Bundling, etc.). Auto-fix first, then verify what remains.

Below the table, 2–4 bullets for non-obvious command behaviour:

- Package manager gotchas (e.g. "never invoke `pip` directly — use `uv run`").
- Lint script composition (e.g. "no `lint:fix` script — pass `--fix` to eslint explicitly").
- Migration command semantics (e.g. "`flask db migrate` only generates a revision; `flask db upgrade` applies").

Each command must be **executable as written** — include flags, not just tool names. The agent will copy-paste these.

### 6. ## Testing

Standalone section, not folded into Commands. Test fixtures and conventions are operational rules (in-memory DB, global mocks, CI triggers), not style — they belong here, not in `STYLE.md`.

Cover:

- Test runner + version, test directory, key fixtures (session-scoped vs per-test, what they truncate, what they clear).
- Frontend test globals and mocks that must be set up in `beforeEach` (e.g. `$route`, `$inertia`, `window.reverseUrl`).
- Test database URL and how it is injected.
- Property-test library availability and what to property-test (pure utilities only — never DB or network code).
- CI trigger (which branch, which suites).

If the repo has no tests, write `## Testing` with a single line: `_No test suite yet._` — do not omit the section. The section's presence is a signal that testing was considered.

### 7. ## Boundaries

Three-tier system, always in this order, always with these exact emoji headings:

- **### ✅ Always** — what every change must do. Application factory, auto-fix + verify before declaring done, update docs when adding/removing modules.
- **### ⚠️ Ask First** — changes that need human approval. CI workflows, non-portable schema changes, intentional non-default tooling choices.
- **### 🚫 Never** — absolute prohibitions. Generated/vendored dirs, migration history edits, secrets, bypassing the application factory, running the scheduler in the web process.

Each bullet is one rule, imperative voice, no hedging. If a rule has a "why", put it in a short clause after an em-dash: `Edit migrations/versions/*.py to "fix" history — write a new revision.`

## Writing rules

- **Direct, imperative voice.** Not "You should consider..." — just "Use `uv run`."
- **Code examples over paragraphs.** One real command beats three sentences describing it.
- **Every command executable as written.** Include `uv run`, `npm run`, flags. No bare `pytest` if the repo needs `uv run pytest`.
- **No "You are an expert..." persona line.** That is persona-file convention. The project-root AGENTS.md describes the repo, not the agent.
- **No general helper preamble.** Skip "This file helps agents work effectively..." — the structure already says that.
- **Point to STYLE.md once, at the top.** Do not duplicate style rules here. If no `STYLE.md` exists, omit the handoff line and note style conventions inline until `STYLE.md` is created.
- **Keep File Structure to significant dirs only.** If removing a dir from the list would not cause an agent to misplace a file, it does not belong here.
- **Length target: 60–120 lines.** If longer, cut prose, not rules. A rule is never padding.

## Anti-patterns to reject

- Using `## Architecture`, `## Environment`, or `## Project Structure` as alternative names for the canonical `## Overview` / `## Tech Stack` / `## File Structure` headings.
- A `## Project` parent heading nesting Overview/Tech Stack/File Structure. Flat top-level sections are canonical and easier for agents to scan.
- A `## Introduction` heading. The intro is a paragraph, no heading.
- Folding Testing into Commands. Testing has fixtures and conventions, not just commands.
- Duplicating style rules from `STYLE.md`. One handoff line, then stop.
- Duplicating human onboarding from `README.md`. No badges, no screenshots, no deploy guide.
- A `## Code Style` section in `AGENTS.md`. That is `STYLE.md`'s job.
- Vague boundaries like "Be careful with migrations". Say the exact rule: `Edit migrations/versions/*.py to "fix" history — write a new revision.`
- Commands without the package manager prefix (`pytest` instead of `uv run pytest`).

## Workflow

1. **Read existing repo docs before writing.** Always read `AGENTS.md`, `README.md`, `STYLE.md` if present. Then scan the repo for other `*.md` files (root, `docs/`, `.github/`) and infer each file's role from its filename and content. Standard docs to recognise:
   - `README.md` — project overview, quick-start, badges
   - `AGENTS.md` — agent instructions (this file)
   - `STYLE.md` / `CODING_STANDARDS.md` — code style, naming, placement
   - `CONTRIBUTING.md` — contribution workflow, PR process
   - `DEPLOYMENT.md` / `DEPLOY.md` — deploy steps, environment provisioning
   - `LICENSE` / `LICENSE.md` — licence terms
   - `CODE_OF_CONDUCT.md` — community conduct
   - `SECURITY.md` — vulnerability reporting policy
   - `CHANGELOG.md` — release history

   For any `*.md` not in this list, read the first heading and opening paragraph to guess its role before deciding whether `AGENTS.md` should reference it. Inventory what already exists so you do not duplicate. Identify what is already canonical and what is duplicated across them.

2. **Inventory the repo** for facts the agent needs: entry point, run command, package manager, test runner, test DB, CI trigger, generated/vendored dirs, scheduler process location. Use `glob`/`grep`/`read`, do not guess.
3. **Draft using the template** in `agents-template.md` (same directory as this skill). Replace every `<...>` placeholder with a real fact from step 2. Delete placeholder rows/sections that do not apply — do not leave `<TODO>` markers.
4. **Check separation of concerns**: any rule that is style (naming, placement, API shape) moves to `STYLE.md`. Any human onboarding (deploy, screenshots) moves to `README.md`. `AGENTS.md` keeps only operational knowledge and boundaries.
5. **Verify**: every command in the table runs as written; every dir in File Structure exists; every boundary rule is imperative and unambiguous.
6. **Always recommend the user restart their agent session** after `AGENTS.md` changes — the running session keeps the old content until restart.

## Template

See `agents-template.md` in this skill's directory. Copy it, fill in the placeholders, delete what does not apply.
