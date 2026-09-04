---
name: project-docs
description: Create, audit, review, refine, or consolidate project Markdown documentation. Use when asked to scaffold, improve, or audit README.md, AGENTS.md, CONTRIBUTING.md, STYLE.md, deployment docs, or a project documentation map; when documentation is stale, outdated, duplicated, missing, or inconsistent with the code; or when asked to review project docs. By default maintains README.md, AGENTS.md, and CONTRIBUTING.md; explicit user scope overrides that default.
---

# Project documentation

Build/audit/refine/consolidate project Markdown from user requirements + existing content only. Manages project Markdown, not arbitrary prose. Does not discover or establish coding policy.

- **Evidence first**: read source, config, tests, CI, deploy files as needed to derive/verify purpose, stack, setup, commands, paths, runtime behavior.
- **Never invent** facts, policies, commands, paths, architecture, workflow. Placeholders ← user-provided facts or existing content only.
- **Don't infer** undocumented policy/style/architecture — ask.
- **Docs files only**: Markdown + `LICENSE` / agent-alias files on user approval.

Modes: **audit** (report ownership/duplication/gaps/stale claims; STYLE.md review = this mode, read-only vs opinionated structure), **scaffold** (missing baseline + justified optional docs), **refine** (keep accurate facts; restructure to template headings when diverged), **consolidate** (dedupe/merge).

## Flow: `CLASSIFY → GATHER → PLAN → ASK → EXECUTE → VERIFY`

- **CLASSIFY**: mode + scope. Named doc/action overrides default scope. No actionable instruction = plan only, no edits. Pick README profile; ask if unclear.
- **GATHER**: in-scope Markdown + matching templates + repo evidence (manifests, lockfiles, config, tests, CI/deploy). Adjacent Markdown (status/migration docs, sub-READMEs) = reference-only, no edits unless in scope. Record missing facts, conflicts, justified optional docs.
- **PLAN**: per templates; keep accurate facts + project policy; drop empty/inapplicable sections. Dedupe operational facts: one owner, others link. Exempt: audience-specific `README.md`/`AGENTS.md` summaries. Pair `STYLE.md`/`AGENTS.md` → owner is `STYLE.md`. Adjacent docs untouched unless in scope. Optional docs only if justified. Audit mode = plan findings.
- **ASK**: one approval request (format below). Blocking questions only: README profile, missing facts, conflicting policy. `lgtm`/`go`/`approve` = whole plan authorized; no second gate. Audit skips ASK.
- **EXECUTE**: approved plan, order: `README.md` → `AGENTS.md` → `CONTRIBUTING.md` → `STYLE.md` → justified optional docs. No unplanned edits.
- **VERIFY** — checklist, not one pass:
  - Update README/AGENTS doc maps on add/remove/rename.
  - Check links, anchors, paths, ownership, README profile.
  - Fact-check ports, lockfiles, versions, commands, paths, test locations vs repo evidence or user-confirmed facts. Unresolved = question/unknown, never guess.
  - Fix false/stale claims in every in-scope doc, including unmodified sections.
  - Commands executable; no placeholder, stale command, fictional policy, empty section.
  - Optional docs justified; dedupe scan on edited docs (README/AGENTS summaries exempt).
  - Report created / updated / unchanged / blocked-needs-user.

## Approval request (ASK) format

One message, four numbered parts:

1. **Scope** — mode, in-scope docs, what won't be touched.
2. **Planned changes** — per doc: create/update/leave + one-line summary.
3. **Defaults to approve** — safe template defaults that would become policy (Principles, Boundaries, branch/commit conventions); user keeps or drops each.
4. **Blocking questions** — README profile, missing facts, conflicting policy; numbered, each with proposed answer.

`lgtm`/`go`/`approve` authorizes all; audit reports findings instead.

## Findings format

Audit + STYLE.md review: one line per finding, grouped by doc, severity order.

- `<location>` — <what is wrong> → <suggested fix> (`stale` | `duplicate` | `gap` | `structure`)

End with items needing user facts. Clean audit says so explicitly. Short: evidence inline, no prose — readable in under a minute.

## Baseline documents

Default scope (named doc = only that doc):

- `README.md` — human entry point.
- `AGENTS.md` — AI coding-agent instructions.
- `CONTRIBUTING.md` — contributor workflow.

## Optional documents

Only when justified. Never invent roadmaps, contact channels, policies, procedures.

| Document             | Create when                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| `STYLE.md`           | Always recommend when code conventions exist; create only on user approval. |
| `DEPLOYMENT.md`      | Deployment/infrastructure/release/recovery steps exist.                     |
| `SECURITY.md`        | Public project or security-sensitive data/access.                           |
| `CHANGELOG.md`       | Consumers need version history; releases not enough.                        |
| `CODE_OF_CONDUCT.md` | Public contributor community.                                               |
| `SUPPORT.md`         | Support channel separate from issues.                                       |
| `ARCHITECTURE.md`    | System design would overload README/STYLE.                                  |
| `GOVERNANCE.md`      | Multiple maintainers need decision rules.                                   |
| `docs/` directory    | Several detailed documents needed.                                          |
| `LICENSE`            | Public/distributed software.                                                |

Agent-tool aliases: some tools read their own file, not `AGENTS.md` (Claude Code → `CLAUDE.md`). On repo evidence, offer one-line redirect file (e.g. `@AGENTS.md`); ask first; follow the tool's real syntax — may be non-Markdown if format requires (e.g. `.cursorrules`).

## README profile

By primary reader/purpose, not repo visibility. Unclear → ask.

### User-facing

For users. Lead with value: name, short description, real badges, screenshot/demo when real; what + who for; usage, install, first use; config/deploy/env notes when needed; links to contribution/license/security/docs when present. TOC only if long. Roadmap/contact/acknowledgments only if real + maintained.

Tech badges: generate at https://badges.pages.dev/, Shields markup, `style=for-the-badge`, `logoColor=white`. Copy real generated URL — never invent slugs/URLs. Only major technologies; link official site when known. Repo-status badges (tag/contributors/forks/stars/issues/license) separate, unchanged.

### Developer-facing

For contributors. Intro sentence = scope + audience. `## Tech Stack` — structural choices only (language, runtime, framework, styling system, database); libraries = dependencies, dev tools = tooling, never stack. `## Prerequisites`. `## Development` = init/bootstrap flow. `## Commands` with `###` subsections as needed (e.g. Quality & Testing with Scope/Auto-fix/Check table). Limitations: context-local `###` subsections preferred; catch-all section only for cross-cutting. `## Documentation` at end. No user-marketing.

## Build AGENTS.md

Owns agent knowledge: repo map, commands, testing behavior, boundaries, generated files, verification requirements. Doc map (table) near top. Link `STYLE.md`/`CONTRIBUTING.md`, don't duplicate. Executable commands, exact paths. `Always` / `Ask First` / `Never` boundaries when applicable.

## Build CONTRIBUTING.md

From `assets/contributing-template.md` + common practice; extend with user policy. Never invent Jira keys, branch prefixes, merge strategy, CLA rules, reviewer rules, support channels, security contacts — ask or use common practice.

## STYLE.md

Encodes maintainer taste — this skill never authors or rewrites its content. Two jobs only; beyond = explicit user request:

- **Scaffold**: empty skeleton from `assets/style-template.md` — section structure + Principles defaults as unconfirmed bullets. Every rule bullet = maintainer's; never fill gaps. Code-convention evidence exists (lint configs, repeated patterns) → scaffold skeleton by default; it has no authored rules, so approval gates normative content, not existence. Opinionated structure below = review checklist, not scaffold content.
- **Review**: check existing STYLE.md vs opinionated structure → findings; read-only, fixes only on request.

Opinionated structure:

- Rule priority present — default Security → Correctness → Architecture → Simplicity → Local consistency; user order fine.
- Ownership declaration present — what is canonical here, what links elsewhere; placement rules don't duplicate `AGENTS.md` repo map.
- No operational facts duplicated from skill-owned docs — link to owner.
- Sections for absent layers/concerns (frontend rules in a CLI), dead sections, stale empty rule bullets in mature STYLE.md = findings. Fresh scaffold placeholders exempt.
- Lint-enforceable rules restated instead of pointing at tool config = findings.

## Ownership + documentation maps

Operational facts: one canonical owner among skill-managed docs. `README.md`/`AGENTS.md` may repeat audience-specific summaries (`AGENTS.md` = README for AI agents). `CONTRIBUTING.md` + others: link to owner, don't copy. `STYLE.md` = SSoT of styling/coding practices, read by humans AND agents: coding-practice fact (conventions, technical contracts, coding constraints, style-related commands) in both `STYLE.md` and `AGENTS.md` → owner is `STYLE.md`; `AGENTS.md` keeps summary or link. Never strip `STYLE.md` in favor of `AGENTS.md`. Adjacent Markdown (status/migration/sub-READMEs): reference-only — fact-check yes, edit/restructure/own/dedupe no, unless in scope; audit may flag stale/boilerplate sub-READMEs as findings.

| Document          | Owns                                                                     |
| ----------------- | ------------------------------------------------------------------------ |
| `README.md`       | Human overview, onboarding, project use.                                 |
| `AGENTS.md`       | Agent workflow, repo map, dev/build/test commands, boundaries.           |
| `CONTRIBUTING.md` | Branches, commits, issues, PRs, review workflow.                         |
| `STYLE.md`        | Styling + coding practices — canonical for humans and agents; architecture rules, technical contracts, coding constraints. Style-related commands/constraints (lint/format, generated-file rules, data-lifecycle contracts) are STYLE.md facts even when `AGENTS.md` states them; `AGENTS.md` commands = dev/build/test workflow. |
| `DEPLOYMENT.md`   | Infrastructure, releases, deployment, recovery.                          |
| `SECURITY.md`     | Vulnerability reporting, security policy.                                |
| `CHANGELOG.md`    | Version history.                                                         |

Doc maps: `AGENTS.md` always (table). `README.md`: dev profile → `## Documentation` at end; user profile → concise linked bullets at end of Getting Started, only when several docs exist. Maps may repeat names/audience, never operational facts. Only existing files; update on add/remove/rename. Examples in templates.

## Writing rules

- Concise readable Markdown: direct language, concrete commands, purpose before detail, labeled examples, local terminology.
- Tables for compact comparisons + AGENTS map; bullet links for README doc list; lists for procedures.
- Cut sections that don't help the reader; code patterns ≠ policy.

## Templates

- `assets/readme-user-template.md`
- `assets/readme-developer-template.md`
- `assets/agents-template.md`
- `assets/contributing-template.md`
- `assets/style-template.md` — reference skeleton for blank projects, not default shape

Templates define expected structure: keep headings + order; replace placeholders with user-provided/existing facts; delete only inapplicable sections. No invented section schemes; deviations need user approval in plan. Never leave `<TODO>` markers. Optional defaults (e.g. `Principles`, `Boundaries`) stay optional until project/user approval, then policy.
