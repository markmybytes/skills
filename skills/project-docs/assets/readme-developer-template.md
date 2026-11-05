<!--
Developer-facing README layout. Gates live per section — remove gated-out
sections entirely. Headings follow the repo language; keep heading ORDER.
Replace {{...}} with user-provided or repo-evidenced facts. Delete this
comment when finished.
-->

# {{project name}}

{{One-sentence project description — what it is, who it is for, and what it owns.}}

<!-- TECH STACK: gate — only structural choices that constrain where and how
code gets written. Versions and full tool lists live in package.json. Each
row = a placement decision. -->

## Tech Stack

| Layer        | Technology               |
| ------------ | ------------------------ |
| {{layer}}    | {{technology}}           |

<!-- PREREQUISITES: gate — only prerequisites a config file cannot answer.
Shape: one line per item — name, one qualifier or link. Never explain how a
version resolves (manifest shows it) and never ship manifest facts (package
manager, lockfile presence). When the manifest names a version manager
(volta, uv, sdkman), it covers the language runtime: list the manager, drop
the bare language bullet. Good items: version manager, VPN, databases,
credentials, language extensions a manager does not cover. -->

## Prerequisites

- {{Tool}} — {{one qualifier or link}}
- {{Tool}} — {{one qualifier or link}}

<!-- DEVELOPMENT: gate — numbered init/bootstrap steps. Setup detail belongs
in CONTRIBUTING.md; link there when the flow is long. -->

## Development

```sh
# 1. Init: clone, configure, install dependencies
{{commands}}

# 2. Bootstrap / first run
{{commands}}
```

<!-- COMMANDS: gate — 2-col table, day-to-day commands. Purpose column =
non-obvious behavior only. Never transcribe what package.json answers.
Agent-facing depth lives in AGENTS.md. -->

## Commands

| Command       | Purpose     |
| ------------- | ----------- |
| `{{command}}` | {{purpose}} |

<!-- DOCUMENTATION: one row per root doc that exists, purpose from its
self-description. AGENTS.md never appears here — human docs do not
reverse-reference it. Delete the section when the list would hold fewer
than three rows. -->

## Documentation

- [DEPLOYMENT.md](DEPLOYMENT.md) — Infrastructure, release process, and deployment details
- [CONTRIBUTING.md](CONTRIBUTING.md) — Branches, commits, and PRs
- [SECURITY.md](SECURITY.md) — Vulnerability reporting, security policy
- [ARCHITECTURE.md](ARCHITECTURE.md) — System design and code placement
- [{{doc}}]({{doc}}) — {{purpose}}
