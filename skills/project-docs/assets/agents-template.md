<!-- Agent instructions template. Keep operational knowledge here, not style policy. -->

# AGENTS.md

<One-sentence project description.>

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |
| <doc>                              | <trigger>           | <purpose / ownership>      |

## Overview

- **Entry:** `<entry point>`
- **Run:** `<development command>`
- **Processes:** `<scheduler, worker, or service notes>`

## Tech Stack

- **<Language/runtime>:** `<version and package manager>`
- **Database:** `<engine and connection note>`
- **Frontend/tooling:** `<framework and config>`
- **Other constraints:** `<timezone, container, or platform note>`

## Repository Map

- `<directory>/` — <navigation purpose>
- `<directory>/` — <navigation purpose>
- `<tests>/` — <test location>

Follow [STYLE.md](STYLE.md) for code placement and architecture rules. Do not
duplicate those rules in this map.

## Commands

Run from repository root. Apply fixes before verification.

| Scope   | Fix         | Verify      |
| ------- | ----------- | ----------- |
| <scope> | `<command>` | `<command>` |

- `<Non-obvious command behavior>`

## Testing

- **<Suite>:** `<runner and version>` — `<test path>`
- **Fixtures/mocks:** `<required setup>`
- **Test database:** `<database and setup>`
- **CI:** `<trigger and suites>`

## Boundaries

<!-- Optional before approval. Present these rules as suggestions; keep only if
the project/user approves them. After approval they become project policy —
remove this note. Replace or remove them with project-specific rules. -->

### ✅ Always

- Read the documentation map and relevant linked docs before making edits.
- Follow explicit user instruction over repository documentation when they conflict.
- Use the exact commands and prerequisites from the docs.
- Run relevant verification or tests before declaring work complete.
- Keep changes scoped and minimal.
- Preserve single sources of truth; update owning docs when behavior or interfaces change.

### ⚠️ Ask First

- Destructive, security-sensitive, or ambiguous changes.

### 🚫 Never

- Commit secrets or local environment values.
- Hand-edit generated, vendored, or protected files.
- <Project-specific prohibition>
