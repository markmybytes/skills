<!--
DEPLOYMENT.md layout. Gates live per section — remove gated-out sections
entirely. Headings follow the repo language; keep heading ORDER. Transcription
rules follow Writing in SKILL.md: configs and workflows self-describe. Delete
this comment when finished.
-->

# {{project name}} — deployment

{{Scope declaration: one line, own scope only — this file owns environments, triggers, release/recovery flow. Values live in workflows and configs; this file says where to look.}}

<!-- ENV MATRIX: core section. One row per environment: what triggers it,
what it points at, non-obvious caveat. Trigger/contract facts only — values
live in the workflow file. -->

## Environments

| Environment | Trigger | Description |
| ----------- | ------- | ----------- |
| {{env}}     | {{trigger}} | {{caveat a config file does not show}} |

<!-- PIPELINE: gate — only when a deploy pipeline or gate exists.
Pipeline stages as pointers to the workflow file; name the file, never
transcribe values. -->

## Pipeline

{{Stages and gates: pointer to `{{.github/workflows/...}}`}}

<!-- VERIFY: gate — only when evidence defines post-deploy checks. Checklist
items = verifiable states with expected results. Values the reader runs or
pastes are allowed per the Writing whitelist. -->

## Verify

- [ ] {{post-deploy check with expected result}}
- [ ] {{health endpoint returns 200}}

<!-- ENVIRONMENT VARIABLES: gate — when deploy-time env or secrets exist.
One bullet per family: name + source pointer (.env.example, vault, CI
settings). Never values — Writing's transcription rules govern. -->

## Environment variables

- {{Variable family}} — {{source pointer}}

<!-- MIGRATIONS: gate — only when deploys run ordered schema or data steps.
State the order and its rollback coupling when evidence shows a multi-step
policy. -->

## Migrations

{{Migration order and rollback coupling}}

<!-- ROLLBACK AND RECOVERY: gate — only when repo evidence shows a real
rollback path. Criteria, the previous artifact pointer (digest or tag), and
the owner. A true failure mode earns the same treatment. Never invent. -->

## Rollback and recovery

{{Rollback criteria, previous-artifact pointer, owner}}
