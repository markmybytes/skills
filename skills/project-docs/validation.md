# project-docs — skill development gate

The gate every skill-content change passes before it becomes a commit. Two
principles hold it together: changes ship with evidence (a blind run of the
changed skill), and every score names an artifact and a command that can
re-run it. This file is development tooling. Never seed it into an
executor's environment — a blind run must not read it.

## Gate

Every change to `SKILL.md` or any template:

1. **SPEC** — the change records: the user decision that motivated it, the
   defect taxonomy it targets, the exact file and clause amended. No spec,
   no commit.
2. **LIVE-FIRE** — one blind dual-agent round on a claim-snapshot of the
   changed skill:
   - Fresh worktree off a fixed base commit. Seed the executor's skill copy
     from the repo source, hash-checked, excluding `validation.md` and any
     record of prior runs (`robocopy /XF validation.md`).
   - **Executor** (fresh agent, no run history): applies the skill to an
     audit-or-scaffold request on the worktree. Blind: never reads prior run
     branches/worktrees, the production reference docs, or this gate's
     records. Holds gate authority, records every gate decision, ships one
     commit, code untouched.
   - **Assessor** (fresh agent, separate session): grades the executor's
     commit only. Conformance: template section order, gates exercised, the
     ownership table, placeholders, scope declarations. Truth: named file
     and line spot-checks via in-tree commands against config, app config,
     workflow, and git-refs. Core promise scored 4 ways: accurate,
     non-duplicated, audience-appropriate, zero-invented.
3. **TRAJECTORY** — the run's report carries the table: conformance %, truth
   %, promise N/4, and the failure taxonomy hit. Every prior fix class
   re-verified as still closed. A repair that reopens an old class = a
   regression, and it is fixed before anything else lands.
4. **VERDICT** — "production-ready pending user pass" requires zero defects
   whose fix calls for code change and residuals only policy-level: writing
   system, tone, per-doc judgment calls the gate records for the user.

One full round per content change. No partial rounds: a one-line wording fix
fails the same gate — that's what keeps wording honest.

## Run record template

Append one block per run:

```
### Run N — <date>
- skill: <commit> · executor: <commit> on <worktree path> · assessor: <session>
- scope: <mode> · gate decisions: <one summary line>
- check rows: <verified N> / unresolved K · catch list: <defects caught>
- conformance <pct> · truth <pct> · promise <N>/4
- verdict: <production-ready pending user pass yes/no>
- repairs landed: <commit or "none">
```

Records live beside the test artifacts, not here. This file holds
methodology only.

