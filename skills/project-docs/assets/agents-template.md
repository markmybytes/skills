<!--
AGENTS.md layout for coding agents. Gates live per section — remove
gated-out sections entirely. Headings follow the repo language; keep heading
ORDER. Keep the finished file at 200 lines or fewer. Delete this comment
when finished.
-->

# {{project name}}

{{Mandated intro: one to three lines on the project's nature. An agent reads it before work. Repo evidence or user input only.}}

<!-- DOC MAP: agent-relevant docs only. A README row and a self-row never
ship — human docs do not reverse-reference this file. One row per doc the
agent must read, purpose = one "defining X" sentence from the doc's own
self-description. Remove rows for files that do not exist. -->

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Before code changes | Architecture and placement |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [{{doc}}]({{doc}})                 | {{trigger}}         | {{purpose}}                |

<!-- COMMANDS: 3-col table. Fix column = autofix only where an autofix
command exists, dash otherwise. Rows must cover install, dev, fix, verify —
the table makes AGENTS.md self-contained. Purpose/scope = repo evidence
(package.json). Never transcribe flags or formulas. Verification commands
named concretely. Remark bullets: behaviors beyond what the commands show.
Never narrate tool chains, CI steps, or hook mechanics — config files own
those. -->

## Commands

Run from repository root. Apply fixes before verification.

| Scope   | Fix         | Verify      |
| ------- | ----------- | ----------- |
| {{scope}} | `{{command}}` | `{{command}}` |

- `{{Non-obvious command-level behavior}}`

<!-- TESTING LADDER: gate — only when the repo has multi-tier test
infrastructure. Ladder = ascending blast radius ("the more a change is
shared, the larger the verification"). Include environment caveat and
fixture facts a config file does not answer. -->

## Testing and verification

- **{{Suite}}:** `{{runner}}` — `{{test path}}`
- **Environment:** `{{required setup, e.g. TZ, DB}}`
- Ladder: {{smallest check first}} → {{largest check}}

<!-- BOUNDARIES: tri-list. Every bullet needs repo evidence (an event,
incident, or convention) or explicit user policy — template defaults ship
never, except the universal secrets bullet below. Ask-first entries come
from user risk policy. Add generated-file and cache dirs the repo builds to
the Never list. Delete guidance comments when finished. -->

## Boundaries

### Always

- {{repo-evidenced rule}}

### Ask first

- {{user risk policy}}

### Never

- Commit secrets or local environment values.
- Hand-edit or regenerate: `{{generated dirs and files}}`, `{{files}}`
- {{repo-evidenced rule}}
