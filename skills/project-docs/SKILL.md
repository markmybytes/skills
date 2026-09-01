---
name: project-docs
description: Create, audit, review, refine, or consolidate project Markdown documentation. Use when asked to scaffold, improve, or audit README.md, AGENTS.md, CONTRIBUTING.md, STYLE.md, deployment docs, or a project documentation map; when documentation is stale, outdated, duplicated, missing, or inconsistent with the code; or when asked to review or update project docs in any way. By default maintains README.md, AGENTS.md, and CONTRIBUTING.md; explicit user scope overrides that default.
---

# Project documentation

Build, audit, refine, or consolidate project Markdown from user-provided requirements and
existing content only. This skill manages project Markdown, not arbitrary prose, and does not
discover or establish coding policy.

- **Ground in evidence**: inspect source, config, tests, CI, and deployment files only as needed
  to derive and verify purpose, stack, setup, commands, paths, and runtime behavior.
- **Never invent** facts, policies, commands, paths, architecture, or workflow — replace
  placeholders only with user-provided facts or existing content.
- **Do not infer** undocumented policy, coding style, architecture, or workflow — ask instead.
- **Markdown only**: never edit non-Markdown files.

Modes: **audit** (report ownership, duplication, gaps, stale claims), **scaffold** (missing baseline and justified optional docs), **refine** (preserve accurate facts), **consolidate** (duplicated or fragmented docs).

## Interaction flow

Use this sequence for every request: `CLASSIFY → GATHER → PLAN → ASK → EXECUTE → VERIFY`

- **CLASSIFY**: Identify mode and scope; a named document or action overrides default baseline scope. No actionable instruction means plan only; do not edit. Determine README profile; ask if unclear.
- **GATHER**: Read in-scope Markdown, matching asset templates, and relevant repository evidence
  (package manifests, lockfiles, root config, test directories, CI/deployment files) to derive
  and verify purpose, stack, setup, commands, paths, and runtime behavior. Adjacent Markdown
  (status, migration docs) is reference-only, not edited unless explicitly in scope. Record
  missing facts, conflicts, and justified optional docs.
- **PLAN**: Plan creation or updates aligned to templates, preserving accurate facts and project
  policy, removing empty/inapplicable sections. Detect duplicated operational facts across
  in-scope, skill-managed docs; choose one owner; replace other copies with links, except
  audience-specific `README.md`/`AGENTS.md` summaries. Not applied to adjacent status or
  migration docs unless in scope. Optional docs only when justified. Audit mode plans findings.
- **ASK**: One approval request covering scope, docs, changes, optional docs, and safe template
  defaults that would become policy. Ask only blocking questions: README profile, missing facts,
  conflicting policy, normative `STYLE.md` rules. `lgtm`/`go`/`approve` authorizes execution; no
  second per-section gate. Audit mode skips approval.
- **EXECUTE**: Apply the approved plan in order: `README.md`, `AGENTS.md`, `CONTRIBUTING.md`, `STYLE.md`, then justified optional docs. No unplanned edits.
- **VERIFY**: Work through a checklist, not one pass:
  - Update README/AGENTS documentation maps on any add, remove, or rename.
  - Verify links, anchors, paths, ownership, and README profile.
  - Fact-check ports, lockfiles, versions, commands, paths, and test locations against
    repository evidence or user-confirmed facts; unresolved claims become questions or
    unknowns, never guesses.
  - Correct false or stale claims in every in-scope document, including sections not
    otherwise modified.
  - Confirm command executability; no placeholder, stale command, fictional policy, or empty
    section remains.
  - Confirm optional docs are justified; scan edited skill-managed docs for duplicated
    operational facts, exempting `README.md`/`AGENTS.md` audience-specific summaries.
  - Report created, updated, unchanged, and blocked/user-needed items.

## Baseline documents

By default, create or maintain these root documents; explicit user scope overrides this default (one named document = edit only that document):

- `README.md` — human project entry point.
- `AGENTS.md` — AI coding-agent instructions.
- `CONTRIBUTING.md` — contributor workflow and standard contribution guidance.

## Optional documents

Recommend or create these only when user or project context justifies them; never create fictional roadmaps, contact channels, policies, or procedures:

| Document             | Create when                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| `STYLE.md`           | Shared coding conventions are substantial or span multiple domains.    |
| `DEPLOYMENT.md`      | Deployment, infrastructure, release, or recovery steps exist.          |
| `SECURITY.md`        | The project is public or handles security-sensitive data or access.    |
| `CHANGELOG.md`       | Consumers need maintained version history and releases are not enough. |
| `CODE_OF_CONDUCT.md` | The project has a public contributor community.                        |
| `SUPPORT.md`         | Users need a support channel separate from issues.                     |
| `ARCHITECTURE.md`    | System design would overload `README.md` or `STYLE.md`.                |
| `GOVERNANCE.md`      | Multiple maintainers need explicit decision rules.                     |
| `docs/`              | Several detailed documents are needed.                                 |
| `LICENSE`            | Public or distributed software needs license terms.                    |

Some coding-agent tools look for their own instruction file instead of `AGENTS.md` (for
example, Claude Code reads `CLAUDE.md`). When repository evidence shows such a tool, offer the
user a one-line alias file that redirects to `AGENTS.md` (for example, `@AGENTS.md`); ask
before creating it, and follow the tool's actual redirect syntax.

## README profile

Choose the profile by the README's primary reader and purpose, not repository visibility. If unclear, ask.

### User-facing README

For project users. Lead with value to a new visitor: project name, short description, useful
badges, demo or screenshot when available; what it does and who it is for; usage, installation,
first use; configuration, deployment, and environment notes when needed; links to contribution,
license, security, support, and detailed docs when present. Table of contents only for a long
README; roadmap, contact, or acknowledgments only when real and maintained.

For technology badges, select from https://badges.pages.dev/ and use generated Shields markup with
default `style=for-the-badge` and `logoColor=white` — copy the real generated URL and markup,
never invent badge slugs or URLs. Include only major technologies central to the project; link
each badge to the technology's official site when known. Keep repository-status badges (tag,
contributors, forks, stars, issues, license) separate and unchanged.

### Developer-facing README

For contributors and developers. Lead with getting a developer productive: project scope and
audience; stack and prerequisites; setup and first run; development, test, quality, and build
commands; important environment or data limitations; links to contribution, style, deployment,
architecture, and agent docs. No user-marketing sections.

## Build AGENTS.md

Create or maintain `AGENTS.md` unless narrower scope is given; it owns agent-relevant knowledge:
repository map, commands, testing behavior, boundaries, generated files, verification
requirements. Include a documentation map near the top; link to `STYLE.md` and `CONTRIBUTING.md`
instead of duplicating them; use executable commands and exact paths; state `Always`, `Ask First`, and `Never` boundaries when applicable.

## Build CONTRIBUTING.md

Start from `assets/contributing-template.md` and common contribution practice; replace or extend
with user-provided policy. Never invent Jira keys, branch prefixes, merge strategy, CLA
requirements, reviewer rules, support channels, or security contacts — ask the user or keep common practice.

## Build or assess STYLE.md

If missing, scaffold from `assets/style-template.md`, ask for project rules, and get approval
before finalizing normative content. If present, refine or consolidate only what the user
requested; treat template common principles as approved defaults after "lgtm"; for "implement"
without details, use existing policy and ask only before adding new normative rules.
`Architecture and Placement` owns placement and ownership rules, does not duplicate the
navigation-only repository map in `AGENTS.md`; split large domain-specific rules into linked docs.

## Ownership and documentation maps

Operational facts have one canonical owner among skill-managed docs. `README.md` and `AGENTS.md`
may repeat audience-specific summaries (`AGENTS.md` is the README for AI agents);
`CONTRIBUTING.md`, `STYLE.md`, and other skill-managed docs must link to the owner instead of
copying operational facts. Adjacent Markdown (status, migration docs) is reference-only: read
for fact checks but never edited, restructured, assigned ownership, or deduplicated unless in scope.

Default ownership:

| Document          | Owns                                                             |
| ----------------- | ---------------------------------------------------------------- |
| `README.md`       | Human overview, onboarding, and project use.                     |
| `AGENTS.md`       | Agent workflow, repository map, commands, tests, and boundaries. |
| `CONTRIBUTING.md` | Branches, commits, issues, pull requests, and review workflow.   |
| `STYLE.md`        | Coding conventions, architecture rules, and technical contracts. |
| `DEPLOYMENT.md`   | Infrastructure, releases, deployment, and recovery.              |
| `SECURITY.md`     | Vulnerability reporting and security policy.                     |
| `CHANGELOG.md`    | Version history.                                                 |

Add a documentation map to `AGENTS.md` always, as a table. In `README.md`, place the map in the
developer onboarding flow (near setup/development) for developer-facing profiles; for
user-facing profiles include it only when several documents exist, as concise linked bullets
under Getting Started. Maps may repeat names and audience for their readers but must not
duplicate operational facts. Include only files that exist; update maps on add, remove, or
rename. Examples live in the templates.

## Writing rules

- Prefer concise, readable Markdown: direct language, concrete commands, purpose before detail,
  representative labeled examples, local terminology.
- Use tables for compact comparisons and the AGENTS.md map; bullet links for the README doc list; lists for procedures.
- Cut sections that do not help the intended reader; do not turn code patterns into policy.

## Templates

- `assets/readme-user-template.md`
- `assets/readme-developer-template.md`
- `assets/agents-template.md`
- `assets/contributing-template.md`
- `assets/style-template.md`

Templates are default structural guides, not rigid output. Replace placeholders with user-provided or existing facts; delete inapplicable sections; never leave `<TODO>` markers. Optional template defaults (for example, `Common Agent Rules` or `Common Principles`) stay optional until project/user approval, then become policy.
