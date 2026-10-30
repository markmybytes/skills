---
name: project-docs
description: >
  Create project documentation from scratch, or audit existing docs and update them in one approved pass. Use when asked to create, scaffold, audit, review, refine, or consolidate project Markdown. In scope: README.md, AGENTS.md, CONTRIBUTING.md, optional SECURITY.md, STYLE.md, DEPLOYMENT.md, ARCHITECTURE.md, CODE_OF_CONDUCT.md, SUPPORT.md. Also use when docs are stale, missing, duplicated, inconsistent with the code, or lack an owning document. Or when asked for a documentation map or ownership cleanup. Manages the root doc set and the docs/ interface only. Specialized docs/ content (ADRs, runbooks, guides) belongs to dedicated skills. Default scope: README.md, AGENTS.md, CONTRIBUTING.md. Explicit user scope overrides.
---

# Project documentation

Create project docs from scratch. Or audit existing docs and update them in one approved pass. Docs stay accurate, non-duplicated, audience-appropriate. Never invent facts. Every operational fact has exactly one owning doc.

Never invent facts, commands, paths, policies, or structure. Fill a `{{...}}` placeholder only from user input, existing content, or repo evidence (manifests, lockfiles, config, tests, CI, deploy files). Ask when the repo does not state the policy. Never infer it.

## Scope

**Tier 0 (always)**: `README.md`, `AGENTS.md`, `CONTRIBUTING.md`. Create by default on scaffold. Audit reports a missing Tier 0 doc as a gap.

**Ask/offer (created only on approval)**: `SECURITY.md`, `STYLE.md`, `DEPLOYMENT.md`, `ARCHITECTURE.md`.

**Tier 2.5 (recommend by default)**: `CODE_OF_CONDUCT.md`, `SUPPORT.md`. Public or contributor-facing repo: propose both in every plan. User drops them at the gate. Create only on approval.

Propose an ask/offer doc only when no doc owns its fact domain. First verify the doc map and skim `docs/` self-descriptions. When `docs/` already covers the facts, link it in the doc map. Ownership is per fact, not per file. A `docs/` deep dive does not own the related root facts. Propose a root doc for unowned facts, linking to `docs/` for detail. Never restate `docs/` facts in a root doc. Never move or fork `docs/` content into a root file. Exception: the user explicitly requests the move. Skill-managed docs live at repo root. Never create one under `docs/`.

**Out of concern**:

- `LICENSE` and `CHANGELOG.md` — never managed. Links and badges may reference `LICENSE`.
- Translated docs — never author. Audit may report translation drift.
- Nested `AGENTS.md` — manage the root set only. Nested files are reference-only. Root/nested contradictions are findings.
- ADR/runbook/guide authoring inside `docs/` — dedicated skills own it.
- Adjacent Markdown, such as status docs and sub-READMEs — reference-only.

**`docs/` = interface only.** Root docs own operational knowledge and facts. `docs/` holds deep dives and narrative, not this skill's concern. Own the doc map and links to `docs/`. Never scaffold the directory. Skim each `docs/` file's title, headings, and stated scope to learn what it covers. Use that metadata for map purposes and domain detection. Never verify `docs/` content against the code. Never judge, edit, or restructure it. Never report its staleness or duplication unless the user explicitly requests it.

## Modes

- **scaffold** — create missing docs: Tier 0 by default, plus Tier 2.5 recommendations and unowned-domain ask/offer proposals.
- **audit → update** — also plain review requests. Collect findings, read-only. Findings cover stale claims, duplicates, gaps, ownership violations, and structural drift from template headings. Findings are the fix plan. One approval executes them, then VERIFY. A plain review request stops at findings. Execute nothing without an explicit fix approval.
- **refine** — fix stale docs. Keep accurate facts. Restructure to template headings when a doc diverged.
- **consolidate** — dedupe and merge across the root set only. Never touch `docs/` content.
- **fast-path** — use when the user names existing doc(s) to fix, no facts are missing, no policy conflicts, and no new defaults need approval. Execute directly. Skip PLAN and ASK. Report after VERIFY. Creating a doc always takes the gate.

## Process

### Flow: CLASSIFY → GATHER → PLAN → ASK → EXECUTE → VERIFY

- **CLASSIFY**: mode, scope, README profile. A named doc or action overrides default scope. No actionable instruction means plan only, no edits.
- **GATHER**: in-scope Markdown, templates, repo evidence. Skim `docs/` self-descriptions only. Record missing facts, policy conflicts, unowned domains.
- **PLAN**: follow templates and ownership rules. Keep accurate facts. Drop empty sections. One owner per fact, others link. Propose ask/offer docs only when unowned.
- **ASK**: the single gate. Fast-path and plain review (findings, no fixes) skip it.
- **EXECUTE**: run the approved plan. Default order: `README.md`, then `AGENTS.md`, then `CONTRIBUTING.md`, then `STYLE.md`, then other approved docs. The plan may reorder. No unplanned edits. Update doc maps on add, remove, or rename.
- **VERIFY** — checklist, not one pass:
  - Verify links, anchors, and paths resolve. Verify the doc map is complete and current.
  - Verify commands, versions, ports, paths, and test locations against repo evidence or facts the user provided. Report unresolved items. Never guess.
  - Fix false claims in every in-scope root doc, including sections you did not change. Never flag `docs/` content.
  - Verify every command is executable. Ship no `{{...}}` fill-in, no fictional policy, no empty section. Exception: a scaffolded `STYLE.md` skeleton keeps its rule fill-ins for the maintainer. Report it as blocked-needs-user. Never invent rules to fill them.
  - Report created, updated, unchanged, and blocked-needs-user items.

### Approval gate (ASK)

One message, four numbered parts:

1. **Scope** — mode, docs in, docs untouched.
2. **Planned changes** — per doc: create, update, or leave, plus one line.
3. **Defaults to approve** — safe template defaults that would become policy (per doc: Principles, Boundaries, branch and commit conventions). User keeps or drops each.
4. **Blocking questions** — README profile, missing facts, policy conflicts. Number them. Propose an answer for each.

`lgtm`, `go`, or `approve` authorizes the whole plan. Approving audit findings uses this same gate.

### Findings

One line per finding. Group by doc. Rank by risk: broken links and wrong commands first, cosmetic gaps last.

- `<location>` — <what is wrong> → <suggested fix> (`stale` | `duplicate` | `gap` | `ownership` | `structure`)

Mark breadth: one line for each surface surveyed but not deeply inspected. End with items that need user facts. State explicitly when an audit is clean. Keep the report readable in under a minute.

## Rules

### Ownership — one owner per fact

| Document          | Owns                                                                    |
| ----------------- | ----------------------------------------------------------------------- |
| `README.md`       | Human overview, onboarding, project use, tech stack.                               |
| `AGENTS.md`       | Agent workflow, overview, repo map, dev/build/test workflow commands, testing, boundaries, canonical doc map. |
| `CONTRIBUTING.md` | Branches, commits, issues, PRs, review workflow.                        |
| `STYLE.md`        | Coding practices SSoT for humans and agents: conventions, code placement, style-related commands (lint/format), generated-file rules. |
| `SECURITY.md`     | Vulnerability reporting, security policy.                               |
| `CODE_OF_CONDUCT.md` | Community conduct rules.                                             |
| `SUPPORT.md`      | Support channels.                                                        |
| `DEPLOYMENT.md`   | Infrastructure, releases, deployment, recovery.                         |
| `ARCHITECTURE.md` | System design.                                                           |

Rules: `README.md` and `AGENTS.md` may repeat audience-specific summaries. `AGENTS.md` is the agent's README. Links follow audience. `CONTRIBUTING.md` points human contributors to `README.md`. Agents reach the same commands from the `AGENTS.md` doc map. `STYLE.md` owns coding facts even when `AGENTS.md` summarizes them. `AGENTS.md` links or summarizes. It never supersedes. Never strip `STYLE.md` in favor of `AGENTS.md`. Doc maps may repeat names and audiences. Never operational facts. Every fact keeps exactly one owner. Other docs link to it. Never copy.

### Placeholders

`{{...}}` marks fill-ins. Replace them with user-provided or repo-evidenced facts. Never ship one, except a scaffolded `STYLE.md` skeleton (see VERIFY). `<...>` marks syntax examples, such as commit formats. Keep them as-is. Never write `<TODO>` markers.

### Writing

Write concise readable Markdown. Use direct language, concrete commands, purpose before detail, local terminology. Use tables for compact comparisons and the AGENTS doc map. Use lists for procedures. Cut sections that do not help the reader. Code patterns are not policy.

## Doc guides

### README profiles

Choose at CLASSIFY by primary reader and purpose, not repo visibility. Ask when unclear.

**User-facing** — open with value: name, short description, real badges, screenshot or demo when real. State what the project does and who it is for. Usage, install, first use, common commands. Config and deploy notes when needed. Link contribution, license, security, conduct, support, and docs when present. TOC only if long.

**Developer-facing** — intro states scope and audience. `## Tech Stack` lists structural choices only: language, runtime, framework, styling system, database. Libraries are dependencies. Dev tools are tooling. Tech Stack never lists either. `## Prerequisites`. `## Development` covers init and bootstrap. `## Commands` summarizes `AGENTS.md` workflow commands. `## Important Notes` summarizes `AGENTS.md` boundaries and environment notes. `## Documentation` ends the file. No user-marketing.

Badges: copy tech badges as generated markup from https://badges.pages.dev/ (Simple Badges) with `style=for-the-badge&logoColor=white`. Include only major technologies. Link each badge to its official site when known. Never invent badge slugs or URLs. Repo-status badges are hand-assembled shields.io URLs. The difference is deliberate. Never normalize one to the other.

### AGENTS.md

The agents.md spec allows freeform Markdown. This skill does not: `assets/agents-template.md` defines the shape. Include: one-line description, doc map, overview, tech stack summary (README owns tech stack), repo map, workflow commands, testing behavior, boundaries, concrete verification commands.

Doc map: one row per root doc, plus every `docs/` file the root set references. Take each purpose from the doc's self-description. Remove rows for files that do not exist.

Style-related commands belong to `STYLE.md`. Link them there. Keep the file at 200 lines or fewer. Nested `AGENTS.md` files are reference-only.

### Aliases

Never create an alias file unprompted. Offer one when repo evidence shows the tool.

Per-tool facts: Claude Code reads `AGENTS.md` natively when no `CLAUDE.md` exists. Use an `@AGENTS.md` redirect only for coexistence. `.cursor/rules/*.mdc` is current. `.cursorrules` is legacy. `.github/copilot-instructions.md` is official. `GEMINI.md` needs config. Never promise imports there.

An existing alias file with content: offer migration. Never overwrite. Follow each tool's real syntax, non-Markdown when required.

### STYLE.md

Propose `STYLE.md` only when convention evidence exists: lint configs or repeated patterns. Create it only through the approval gate. Never auto-create. Offer no authored rules beyond `assets/style-template.md` safe-defaults (Principles, Rule Priority). Every rule bullet is the maintainer's. Rewrite `STYLE.md` content only on explicit user request.

Review of an existing `STYLE.md` is audit mode, against the checklist below. Never verify it against the template skeleton. The skeleton is reference only. Checklist: rule priority present. Ownership declaration present. No operational facts duplicated. No sections for absent layers. No dead sections. No empty rule bullets (fresh scaffold placeholders exempt). No restated lint-enforceable rules (reference the tool config instead).

### CONTRIBUTING.md

Never invent Jira keys, branch prefixes, merge strategy, CLA rules, reviewer rules, support channels, or security contacts. Ask or use common practice. The template carries the structure.

## Templates

Templates define expected structure. Keep headings and order, except `assets/style-template.md` (reference skeleton — verify `STYLE.md` against its review checklist). Replace `{{...}}`. Remove inapplicable sections.

- `assets/readme-user-template.md`
- `assets/readme-developer-template.md`
- `assets/agents-template.md`
- `assets/contributing-template.md`
- `assets/style-template.md`

For ask/offer docs beyond these templates, follow public conventions, such as the `SECURITY.md` disclosure pattern. Never invent structure.
