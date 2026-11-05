---
name: project-docs
description: Create project documentation from scratch, or audit existing docs and update them in one approved pass. Use when asked to create, scaffold, audit, review, refine, or consolidate project Markdown. In scope: README.md, AGENTS.md, CONTRIBUTING.md, optional SECURITY.md, STYLE.md, DEPLOYMENT.md, ARCHITECTURE.md, CODE_OF_CONDUCT.md, SUPPORT.md. Also use when docs are stale, missing, duplicated, inconsistent with the code, or lack an owning document. Or when asked for a documentation map or ownership cleanup. Not for code review or prose quality. Manages the root doc set and the docs/ maps-and-links surface only. Specialized docs/ content (ADRs, runbooks, guides) belongs to dedicated skills. Default scope: README.md, AGENTS.md, CONTRIBUTING.md. Explicit user scope overrides.
---

# Project documentation

Create project docs from scratch. Or audit existing docs and update them in one approved pass. Docs stay accurate, non-duplicated, audience-appropriate. Never invent facts. Every operational fact has exactly one owning doc.

Never invent facts, commands, paths, policies, or structure. Fill a `{{...}}` placeholder only from user input, existing content, or repo evidence (manifests, lockfiles, config, tests, CI, deploy files). Ask when the repo does not state the policy. Never infer it.

## Scope

**Tier 0 (always)**: `README.md`, `AGENTS.md`, `CONTRIBUTING.md`. Create by default on scaffold. Audit reports a missing Tier 0 doc as a gap.

**Ask/offer (created only on approval)**: `SECURITY.md`, `STYLE.md`, `DEPLOYMENT.md`, `ARCHITECTURE.md`.

**Recommend-list (propose by default)**: `CODE_OF_CONDUCT.md`, `SUPPORT.md`. Propose both in every plan for a public or contributor-facing repo — evidence: a public remote or contributor-facing docs. No such evidence: it is a gate part 4 question, never a guess. User drops them at the gate. Create only on approval.

Propose an ask/offer doc only when no doc owns its fact domain and repo evidence backs it (convention, config, or pipeline signals). First verify the doc map and skim `docs/` self-descriptions. Ownership is per fact, not per file. Skill-managed domain knowledge — style, constraints, architecture — found in `docs/`, coherent or scattered, centralizes into its owning root doc by default. Propose the centralization as an audit finding: name the fact domain, list involved `docs/` files by name plus self-description, then at the gate move the facts into the root doc, verify them against repo evidence, and the user removes the `docs/` source. Scattered fragments on one domain = one `structure` finding listing them by name and self-description. A `docs/` doc claiming a domain a root doc also claims = an `ownership` finding. Narrative content — ADRs, guides, logs — stays in `docs/`, untouched. The skill never moves or forks `docs/` content silently. A gate-approved centralization counts as the explicit request. Skill-managed docs live at repo root. Never create one under `docs/`.

A doc earns existence by a coherent fact family plus evidence-backed rules that no config or existing doc carries. Below that, fold the facts into the doc that already owns the neighboring ones. Never split on one signal alone.

**Out of concern**:

- `LICENSE` and `CHANGELOG.md` — never managed. Links and badges may reference `LICENSE`.
- Translated docs — never author. Audit may report translation drift.
- Nested `AGENTS.md` — manage the root set only. Nested files are reference-only. Root/nested contradictions are findings.
- ADR/runbook/guide authoring inside `docs/` — dedicated skills own it.
- Adjacent Markdown, such as status docs and sub-READMEs — reference-only.

**`docs/` = maps and links only.** Root docs own operational knowledge and facts. `docs/` holds deep dives and narrative, not this skill's concern. Own the doc map and links to `docs/`. Never scaffold the directory. Skim each `docs/` file's title, headings, and stated scope to learn what it covers. Use that metadata for map purposes, domain detection, and finding generation. Never verify `docs/` content against the code. Never judge, edit, or restructure it. Never report its staleness or duplication. Exception: the user explicitly requests it.

## Modes

- **scaffold** — create missing docs: Tier 0 by default, plus recommend-list suggestions and evidence-backed ask/offer proposals.
- **audit → update** — also plain review requests. Collect findings, read-only. Findings cover stale claims, duplicates, gaps, ownership violations, structural drift from template headings, and `docs/` centralization candidates. Findings are the fix plan. One approval executes them, then VERIFY. A plain review request stops at findings. Execute nothing without an explicit fix approval.
- **refine** — fix stale docs. Keep accurate facts. Restructure to template headings only when the user asked or approved the restructure — local naming can be intentional. A section or contract present in the source doc drops only with named disproving evidence. Without it, keep it or move it to its owner.
- **consolidate** — dedupe and merge across the root set only. Never touch `docs/` content. A consolidation request involving `docs/` = an audit→update plan with centralization findings instead.
- **fast-path** — not a pre-mode: a branch after GATHER. Conditions: the user named existing doc(s), gathered facts are complete, no policy conflicts, no new defaults need approval. Then skip PLAN and ASK, execute, report after VERIFY. Mid-run discovery of a missing fact or a policy need: stop, route through the gate. Creating a doc always takes the gate.

## Process

### Flow: CLASSIFY → GATHER → PLAN → ASK → EXECUTE → VERIFY

- **CLASSIFY**: mode, scope, README profile. A named doc or action overrides default scope. A doc the user names enters the plan as requested — the gate still runs for its defaults and blocking facts. Unclear profile is a gate part 4 question, not a side ask. No actionable instruction means plan only, no edits.
- **GATHER**: in-scope Markdown, templates, repo evidence. Skim `docs/` self-descriptions only. Mine history and command surfaces before drafting workflow conventions: `git log --format=%s` for tracker keys and merge suffixes, the CLI registry (such as console commands) for the command surface. Classify every fact candidate before writing: a repo file or the framework docs already state it → pointer only. Decision reasons and contracts → docs may own. Record missing facts, policy conflicts, unowned domains.
- **PLAN**: follow templates, gates, and ownership rules. Keep accurate facts. Remove empty sections. One owner per fact, others link. Propose ask/offer docs only when unowned and repo-evidenced. Feedback across multiple files: consolidate into one per-doc plan before editing.
- **ASK**: the single gate. Fast-path and plain review (findings, no fixes) skip it.
- **EXECUTE**: run the approved plan. A fast-path run's named fixes are the plan. Default order: `README.md`, then `AGENTS.md`, then `CONTRIBUTING.md`, then other approved docs. The plan may reorder. No unplanned edits. Update doc maps on add, remove, or rename.
- **VERIFY** — checklist — loop until clean:
  - Verify links, paths, and the doc map resolve and stay current. Verify referenced config filenames exist.
  - Verify commands, versions, ports, paths, and test locations against repo evidence or facts the user provided. Report unresolved items. Never guess. A claim that is unverifiable but not disproven lands as a pointer where a source exists; with no source, drop it and report blocked-needs-user.
  - Claims carried from an existing doc are not evidence. List every moved or repeated factual claim as a check row — prerequisites, commands, environment behavior, file presence. Verify each row against its config, code, or file existence. A scan without rows does not count. Never flag `docs/` content. A command row that ships in more than one doc reconciles against its single manifest source; wording drift between docs is a failing row.
  - Fix false claims in every in-scope root doc, including sections you did not change.
  - Sweep every edited doc for transcription residue, double-owned facts, duplicate or empty headings. Semantic repeats count: the same claim rephrased across docs is a double owner even when no two lines match textually.
  - A count or statistic carries its verifying command. A count that is not re-derivable in-tree gets re-derived before it ships or is replaced by a qualitative statement. Never ship a number that survives only because a source doc carried it.
  - Ship no `{{...}}` fill-in, no fictional policy, no empty section. Exceptions: a scaffolded `STYLE.md` or a conventions section folded into `ARCHITECTURE.md` keeps rule fill-ins the user deferred; contact fill-ins in `SECURITY.md`, `CODE_OF_CONDUCT.md`, `SUPPORT.md` may ship when no contact exists. Both report as blocked-needs-user. Never invent rules.
  - Report created, updated, unchanged, and blocked-needs-user items. Report check-row counts: verified N with the commands that verified them, unresolved K.

### Approval gate (ASK)

One message, four numbered parts:

1. **Scope** — mode, docs in, docs untouched.
2. **Planned changes** — per doc: create, update, or leave, plus one line.
3. **Defaults to approve** — safe template defaults that would become policy (per doc: branch and commit conventions). User keeps or drops each.
4. **Blocking questions** — README profile, repo-wide writing system when existing docs use more than one script (surface the prior docs' script and the user-base locale as candidates), missing facts, policy conflicts, fold-or-split choices. Number them. Propose an answer for each.

`lgtm`, `go`, or `approve` authorizes the whole plan. Approving audit findings uses this same gate.

### Findings

One line per finding. Group by doc. Rank by risk: broken links and wrong commands first, cosmetic gaps last.

- `<location>` — <what is wrong> → <suggested fix> (`stale` | `duplicate` | `gap` | `ownership` | `structure`)

Mark breadth: one line for each surface surveyed but not deeply inspected. End with items that need user facts. State explicitly when an audit is clean. Keep the report readable in under a minute.

## Rules

### Ownership — one owner per fact

| Document          | Owns                                                                    |
| ----------------- | ----------------------------------------------------------------------- |
| `README.md`       | Human overview, onboarding, positioning, tech stack as structural choices. |
| `AGENTS.md`       | Agent intro, agent-relevant doc map, all workflow commands (workflow, lint/format), testing ladder, boundaries, generated-file bans. Coding conventions fold here when no `STYLE.md` exists. |
| `CONTRIBUTING.md` | Branches, commits, PRs, review workflow.                                 |
| `ARCHITECTURE.md` | Repo structure deltas, code placement, domain contracts, system design.  |
| `DEPLOYMENT.md`   | Environments, triggers, pipeline, release, recovery.                     |
| `SECURITY.md`     | Vulnerability reporting, security policy.                               |
| `STYLE.md`        | Conditional coding-conventions doc when the family earns it with evidence-backed rules. |
| `CODE_OF_CONDUCT.md` | Community conduct rules.                                             |
| `SUPPORT.md`      | Support channels.                                                        |

The table is the default proposal, not policy. Reassigning an owner, dissolving a doc, or folding one into another is a gate decision. Reassignment executes as: move the facts to the new owner. At the old spot, one redirect line to the new owner — the single exception to the neighbor-pointer ban. Dissolving a doc = move every fact into its absorber, delete the file, and remove its map rows. Never block a user-requested reassignment. Scope and evidence rules stay.

Rules: `README.md` and `AGENTS.md` are the two map holders; every other root doc is a content doc. Every fact keeps exactly one owner. Other docs link to it — never copy it elsewhere. Allowed repeats: `README.md` and `AGENTS.md` may restate audience-specific summaries. Prose summaries stay one line. Command tables may repeat rows across both — two audiences. Longer retranscription never ships. Links follow audience. `CONTRIBUTING.md` points human contributors to `README.md`. Agents use `AGENTS.md`. Human docs never reverse-reference `AGENTS.md`. A scope declaration = one line, own scope only, required in every content doc. Neighbor pointers ("X lives in Y.md") live only in the map holders — except the reassignment redirect above.

### Placeholders

`{{...}}` marks fill-ins. Replace them with user-provided or repo-evidenced facts. Never ship one. Exceptions: rule fill-ins the user deferred in a conventions doc (see VERIFY), and contact fill-ins in `SECURITY.md`, `CODE_OF_CONDUCT.md`, `SUPPORT.md` when no contact exists (report blocked-needs-user). `<...>` marks syntax examples, such as commit formats. Keep them as-is. Never write `<TODO>` markers.

### Writing

Docs follow the repo's language. Headings and body in repo language. Keep technical names, commands, and file names verbatim. One writing system across the doc set. Existing docs in more than one script = a gate part 4 question, never a per-doc inference.

Cross-doc references use real file links. Raw section anchors never: they rot on rename and render differently per viewer. Same-document TOC anchors are fine.

Imperative voice with rationale: state the why once, then the rule. Checklist items state verifiable states, never advice. Explore before asking: answer from the repo first, ask only when still ambiguous. Examples that claim repo evidence resolve to a real branch, commit, or file. Unverifiable ones ship as `<example>` with the evidence claim removed.

Never transcribe what a config file can answer. Hosts, ports, secret names, image tags, framework-official semantics, engine-version formulas — configs self-describe, pointer instead. Versions: quote the manifest constraint verbatim or ship the name without a number. Whitelist: values the reader copies at run time (commands, URLs) and load-bearing contract numbers. Never re-add sections the user dropped. Mechanism sentences — delivery, deploy paths, pipeline steps — cite the workflow's own step names and parameters verbatim, such as suite and scope values. Step names differ per environment; quote each exact name or point to the workflow file instead of asserting a same-name claim. Never paraphrase a mechanism.

Write concise readable Markdown. Use tables for compact comparisons and doc maps. Use lists for procedures. Cut sections that do not help the reader. Do not promote code patterns into doc rules. Exception: an owned contract or pitfall rule that the owning doc's evidence gate backs.

## Doc guides

Policy per doc. Layout lives in the template files — structure and gates there, policy here.

### README profiles

Two templates: `assets/readme-user-template.md` (human onboarding) and `assets/readme-developer-template.md` (project orientation). Choose at CLASSIFY by primary reader and purpose, not repo visibility.

Tech stack = structural choices that constrain where and how code gets written. Versions and full tool lists never ship — `package.json` owns them.

Prerequisites shape: one line per item — name plus one qualifier or link. Keep a qualifier only when the tool's intent is not guessable from its name or link; a well-known tool ships as a bare link. Never explain how a version resolves — the manifest shows it. Never ship manifest facts. A version manager covers the runtime it manages: list it, drop the bare language bullet.

Badges: copy tech badges as generated markup from https://badges.pages.dev/ (Simple Badges) with `style=for-the-badge&logoColor=white`. Include only major technologies. Link each badge to its official site when known. Never invent badge slugs or URLs. Repo-status badges are hand-assembled shields.io URLs. Never normalize one style to the other.

The documentation map lists the root docs plus `docs/migration/` only. Other directory rows belong to the owning map (AGENTS.md or the doc that owns them), never README.

### AGENTS.md

Template: `assets/agents-template.md`.

The intro is mandated: one to three lines on the project's nature. An agent reads it before work. The doc map lists agent-relevant docs only — a README row and a self-row never ship. Commands live in one table and must make AGENTS.md self-contained: install, dev, fix, verify — no doc-map hop to README. Command remarks list only behaviors beyond what the commands show. Never narrate tool chains, CI internals, or hook mechanics — configs own them. The testing ladder ships only when the repo has multi-tier test infrastructure. Every boundaries bullet needs repo evidence or user policy — template defaults never ship, except the universal secrets rule.

### CONTRIBUTING.md

Template: `assets/contributing-template.md`.

Never invent Jira keys, branch prefixes, merge strategy, CLA rules, reviewer rules, support channels, or security contacts. Ask or use repo evidence. An Issues section ships only when the repo uses GitHub Issues. Workflow rules stay mechanism-neutral: state the rule, never name the enforcement layer. The traceability anchor (the repo's tracker key) rides branch and PR title per repo policy. Commit-message grammar defaults to Conventional Commits; repo evidence of a different grammar replaces the default at the gate. Checklist items are verifiable states with the why stated once. A sibling repo's CONTRIBUTING the user names as a model: adopt its structure and workflow mechanics, route every fact to its owner per the table.

### ARCHITECTURE.md

Template: `assets/architecture-template.md`.

Owns repo-structure deltas (standard framework layout gets one pointer line, never a transcription), code placement, per-domain contracts, and system design. Code placement = directory placement; data-lifecycle and routing rules live with their contracts. Holds coding conventions when no `STYLE.md` exists. Gate per contract section: official docs do not say it, and a change here breaks things. Pitfall-plus-reason lines are the content. Neighbor pointers never ship here — one scope-declaration line is required. A coexisting structure doc (`STRUCTURE.md` or a variant) is an `ownership` finding: structure is this doc's fact family. Audit proposes the fold at the gate; consolidate executes it in the root set.

### DEPLOYMENT.md

Template: `assets/deployment-template.md`.

Core content = the environment trigger matrix: deploy facts a workflow file cannot self-describe. Everything it can self-describe follows the transcription rules in Writing. The template's gated sections — post-deploy checks, rollback criteria with a previous-artifact pointer, environment-variable names with source pointers, migration order — carry the rest. These docs usually live as a docs-tree page; this skill manages one at repo root per the centralization rule.

### Aliases

Never create an alias file unprompted. Offer one when repo evidence shows the tool.

Per-tool facts: Claude Code reads `AGENTS.md` natively when no `CLAUDE.md` exists. Use an `@AGENTS.md` redirect only for coexistence. `.cursor/rules/*.mdc` is current. `.cursorrules` is legacy. `.github/copilot-instructions.md` is official. `GEMINI.md` needs config. Never promise imports there.

An existing alias file with content: offer migration. Never overwrite. Follow each tool's real syntax, non-Markdown when required.

### STYLE.md

Propose only when a coherent convention family carries evidence-backed rules that no config or existing doc carries; otherwise fold into `ARCHITECTURE.md` or `AGENTS.md`. Create only through the approval gate. Never auto-create. Every rule bullet is the maintainer's. Rewrite `STYLE.md` content only on explicit user request.

Template: `assets/style-template.md`. Review of an existing one is audit mode: scope declaration present, one line, no neighbor pointers. No config-transcribed rules — point at the tool config. No framework-official semantics. No dead sections. No empty rule bullets (fresh scaffold placeholders exempt). A gate-approved centralization counts as the explicit request for `STYLE.md` content changes.

## Templates

Templates own shape and gates. This file owns policy. Keep section order. Follow each template's gate comments: remove gated-out sections entirely, replace `{{...}}`, write section headings in repo language.

- `assets/readme-user-template.md`
- `assets/readme-developer-template.md`
- `assets/agents-template.md`
- `assets/contributing-template.md`
- `assets/architecture-template.md`
- `assets/deployment-template.md`
- `assets/style-template.md`

Ask/offer docs beyond these templates follow public conventions, such as the `SECURITY.md` disclosure pattern. Never invent structure.
