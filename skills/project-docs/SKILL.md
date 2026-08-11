---
name: project-docs
description: Create, audit, refine, or consolidate project Markdown documentation. Use when asked to scaffold README.md, AGENTS.md, CONTRIBUTING.md, STYLE.md, deployment docs, or a project documentation map. By default maintains README.md, AGENTS.md, and CONTRIBUTING.md; explicit user scope overrides that default.
---

# Project documentation

Build, audit, refine, or consolidate project docs from user-provided
requirements and existing document content only. This skill manages project
Markdown, not arbitrary prose. It does not discover or establish coding policy,
does not inspect source, tests, config, CI, or deployment files, and does not
infer style from the repository. Keep each fact in one canonical document;
link to it instead of copying details. Never invent facts, policies, commands,
paths, architecture, or workflow — replace placeholders only with
user-provided facts or existing content.

It can **audit** existing Markdown (report ownership, duplication, gaps, stale
claims), **scaffold** missing baseline and justified optional documents,
**refine** an existing document preserving accurate facts, and **consolidate**
duplicated or fragmented documentation.

## Interaction flow

- **No actionable instruction**: do not edit. Present a plan covering baseline
  documents, optional documents, and the README profile; ask whether this is a
  Public Project or Team Onboarding README.
- **Explicit action** (audit, refine, consolidate): follow that scope; if the
  prompt names one document, edit only that document.
- **Existing Markdown**: align in-scope documents to their matching template
  structure by default, preserving accurate facts and local policy unless
  stale, contradictory, or user-changed; drop empty or inapplicable sections.
- **Plan approved** (go / lgtm / approve): proceed within the confirmed scope;
  show safe baseline rules from the templates as suggestions — approved rules
  become project policy and optional labels are removed.
- Ask only blocking questions (README profile, missing project facts,
  conflicting policy, or approval for normative `STYLE.md` rules); after edits,
  update documentation maps and verify links and placeholders.

## Baseline documents

By default, create or maintain these root documents; explicit user scope
overrides this default (one named document = edit only that document):

- `README.md` — human project entry point.
- `AGENTS.md` — AI coding-agent instructions.
- `CONTRIBUTING.md` — contributor workflow and standard contribution guidance.

## Optional documents

Recommend or create these only when user or project context justifies them;
never create fictional roadmaps, contact channels, policies, or procedures:

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

## README profile

Use the repository's visibility and audience. If unclear, ask.

### Public Project README

For open-source software or public tools. Lead with value to a new visitor:
project name, short description, useful badges, and demo or screenshot when
available; what it does and who it is for; installation and first use;
configuration and deployment when needed; links to contribution, license,
security, support, and detailed docs when present. Add a table of contents
only for a long README; roadmap, contact, or acknowledgments only when real
and maintained.

For technology badges, select from https://badges.pages.dev/ and use the
generated Shields markup with default `style=for-the-badge` and
`logoColor=white` — copy the real generated URL and markup, never invent badge
slugs or URLs. Include only major technologies central to the project, and
link each badge to the technology's official site when known. Keep
repository-status badges (tag, contributors, forks, stars, issues, license)
separate and unchanged.

### Team Onboarding README

For private, internal, or team-maintained software. Lead with getting a
developer productive: project scope and audience; stack and prerequisites;
setup and first run; development, test, quality, and build commands; important
environment or data limitations; and links to contribution, style, deployment,
architecture, and agent docs. No public-marketing sections.

## Build AGENTS.md

Unless the user gives narrower scope, create or maintain `AGENTS.md` for
AI-assisted repositories; it owns agent-relevant knowledge: repository map,
commands, testing behavior, boundaries, generated files, and verification
requirements. It must include a documentation map near the top; point to
`STYLE.md` for coding rules and `CONTRIBUTING.md` for contribution workflow
instead of duplicating them; keep a short repository map for navigation; use
executable commands and exact paths; and state `Always`, `Ask First`, and
`Never` boundaries when applicable.

## Build CONTRIBUTING.md

Start from `assets/contributing-template.md` and common contribution practice;
replace or extend with user-provided project policy. The template already
covers branching, commits, pull requests, and issues. Never invent Jira keys,
branch prefixes, merge strategy, CLA requirements, reviewer rules, support
channels, or security contacts — ask the user or keep common practice.

## Build or assess STYLE.md

If `STYLE.md` is missing, scaffold it from `assets/style-template.md`, ask the
user for project rules, and get approval before finalizing normative content.
If it exists, refine or consolidate only the content the user requested. Treat
the template's common principles as approved defaults after "lgtm"; for
"implement" without details, use existing policy and ask only before adding
new normative rules. `Architecture and Placement` owns rules: where code
belongs, ownership boundaries, and when to split or combine files; it does not
duplicate the navigation-only repository map in `AGENTS.md`; split large
domain-specific rules into separate linked documents rather than one
unscannable file.

## Ownership and documentation maps

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

Add a documentation map to both `README.md` and `AGENTS.md`: maps may repeat
names and audience for their readers, but must not duplicate operational
facts. `README.md` uses concise linked bullets; `AGENTS.md` uses a table.
Include only files that exist; update both maps on add, remove, or rename.

README map (concise linked bullets):

```markdown
## Documentation

- [DEPLOYMENT.md](DEPLOYMENT.md) — Infrastructure, release process, and deployment details
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
```

AGENTS map:

```markdown
## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |
```

## Verification

- Every command is executable as written or clearly marked as an example; every
  linked file, anchor, and documented path exists.
- Facts have one owner, except audience-specific README/AGENTS summaries.
- No placeholder, stale command, fictional policy, or empty section remains;
  README profile matches audience; optional documents are justified.

## Writing rules

- Prefer concise, readable Markdown: direct language, concrete commands,
  purpose before detail, representative labeled examples, local terminology.
- Use tables for compact comparisons and the AGENTS.md map; use bullet links
  for the README documentation list and lists for procedures.
- Cut sections that do not help the intended reader; do not turn code patterns
  into policy.

## Templates

Use templates in `assets/` as default structural guides, not rigid output:

- `assets/readme-public-template.md`
- `assets/readme-team-template.md`
- `assets/agents-template.md`
- `assets/contributing-template.md`
- `assets/style-template.md`

Replace placeholders with user-provided or existing facts; delete inapplicable
sections; never leave `<TODO>` markers in a finished document. Optional
default rules in templates (for example, `Common Agent Rules` or `Common
Principles`) stay optional until project/user approval, then become policy.
