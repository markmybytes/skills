<!-- Developer-facing README template. Keep it practical; omit user-marketing sections. -->

# <Project name>

<One-sentence project description — what it is, who it is for, and what it owns.>

## Tech Stack

<!-- Load-bearing structural choices only: language, runtime, framework,
styling system, database. Incidental libraries are dependencies and dev
tools are tooling — list them under Prerequisites/Commands, never here. -->

| Layer        | Technology                     |
| ------------ | ------------------------------ |
| <Layer>      | <Technology and version>       |

## Prerequisites

- <Required runtime and version>
- <Required services or credentials>

## Development

```<shell>
# 1. <Init: clone, configure, install dependencies>
<commands>

# 2. <Bootstrap / first run>
<commands>
```

## Commands

<!-- One section for day-to-day commands; add ### subsections only as
needed (e.g. Dev Server, Quality & Testing, Generated Files). -->

| Command     | Purpose   |
| ----------- | --------- |
| `<command>` | <Purpose> |

### Quality & Testing

<!-- Run auto-fix tools first, then check tools for remaining issues. -->

| Scope    | Auto-fix | Check |
| -------- | -------- | ----- |
| <Scope>  | <cmd>    | <cmd> |

<Test database, fixture, environment, or CI notes that developers need.>

## Important Notes

- <Environment, generated-file, data, or local-development limitation>

## Documentation

<!-- Keep only documents that exist; each linked document owns its details. -->

- [DEPLOYMENT.md](DEPLOYMENT.md) — Infrastructure, release process, and deployment details
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents
- [STYLE.md](STYLE.md) — Coding conventions
