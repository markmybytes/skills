<!--
Agent instructions template. Operational knowledge only — style rules live in
STYLE.md. Keep the finished file at 200 lines or fewer. Delete this comment
when finished.
-->

# AGENTS.md

{{One-sentence project description.}}

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |
| [{{doc}}]({{doc}})                 | {{trigger}}         | {{purpose / ownership}}    |

<!-- Add a row for every root doc plus every docs/ file the root set
references — purpose from the doc's own self-description. Delete rows for
files that do not exist. -->

## Overview

- **Entry:** `{{entry point}}`
- **Run:** `{{development command}}`
- **Processes:** `{{scheduler, worker, or service notes}}`

## Tech Stack

<!-- Summary. README owns tech stack. -->

- **{{Language/runtime}}:** `{{version and package manager}}`
- **Database:** `{{engine and connection note}}`
- **Frontend/tooling:** `{{framework and config}}`
- **Other constraints:** `{{timezone, container, or platform note}}`

## Repository Map

- `{{directory}}/` — {{navigation purpose}}
- `{{tests}}` — {{test location}}

Follow [STYLE.md](STYLE.md), when present, for code placement and architecture rules; do not
duplicate them here.

## Commands

Run from repository root. Workflow commands only (dev/build/test). Style
commands (lint/format autofix) are STYLE.md facts — link to its Commands and
Tooling section, never park them here; if STYLE.md is absent and lint evidence
exists, propose it. The Fix column covers build/test repair only. Apply fixes before verification.

| Scope   | Fix         | Verify      |
| ------- | ----------- | ----------- |
| {{scope}} | `{{command}}` | `{{command}}` |

- `{{Non-obvious command behavior}}`

## Testing

- **{{Suite}}:** `{{runner and version}}` — `{{test path}}`
- **Fixtures/mocks:** `{{required setup}}`
- **Test database:** `{{database and setup}}`
- **CI:** `{{trigger and suites}}`

## Boundaries

<!-- Optional before approval. Present these rules as suggestions; keep only
if the project/user approves them. After approval they become project policy —
remove this note. Replace or remove with project-specific rules. -->

### Always

- Read the documentation map and relevant linked docs before making edits.
- Follow explicit user instruction over repository documentation when they conflict.
- Use the exact commands and prerequisites from the docs.
- Run relevant verification or tests before declaring work complete.
- Preserve single sources of truth; update owning docs when behavior or interfaces change.

### Ask First

- Destructive, security-sensitive, or ambiguous changes.

### Never

- Commit secrets or local environment values.
- Hand-edit generated, vendored, or protected files.
- {{Project-specific prohibition}}
