<!-- Developer-facing README template. Keep it practical; omit user-marketing
sections. Delete this comment when finished. -->

# {{project name}}

{{One-sentence project description — what it is, who it is for, and what it owns.}}

## Tech Stack

<!-- Load-bearing structural choices only: language, runtime, framework,
styling system, database. Incidental libraries are dependencies and dev tools
are tooling — list them under Prerequisites/Commands, never here. -->

| Layer        | Technology               |
| ------------ | ------------------------ |
| {{layer}}    | {{technology and version}} |

## Prerequisites

- {{Required runtime and version}}
- {{Required services or credentials}}

## Development

```sh
# 1. Init: clone, configure, install dependencies
{{commands}}

# 2. Bootstrap / first run
{{commands}}
```

## Commands

<!-- One section for day-to-day commands; add ### subsections only as needed
(e.g. Dev Server, Quality & Testing, Data Setup). Audience summary of
AGENTS.md workflow commands — link there for agent-facing detail. -->

| Command     | Purpose   |
| ----------- | --------- |
| `{{command}}` | {{purpose}} |

### Quality & Testing

<!-- Auto-fix tools first, then check tools for remaining issues.
Style-related commands belong to STYLE.md; link if listed. -->

| Scope    | Auto-fix | Check |
| -------- | -------- | ----- |
| {{scope}} | `{{cmd}}` | `{{cmd}}` |

{{Test database, fixture, environment, or CI notes that developers need.}}

## Important Notes

- {{Environment, generated-file, data, or local-development limitation}}

## Documentation

<!-- Keep only documents that exist; add a row for every docs/ file the root
set references, purpose from its self-description. -->

- [DEPLOYMENT.md](DEPLOYMENT.md) — Infrastructure, release process, and deployment details
- [CONTRIBUTING.md](CONTRIBUTING.md) — Branches, commits, and PRs
- [SECURITY.md](SECURITY.md) — Vulnerability reporting, security policy
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents
- [STYLE.md](STYLE.md) — Coding conventions
- [{{doc}}]({{doc}}) — {{purpose}}
