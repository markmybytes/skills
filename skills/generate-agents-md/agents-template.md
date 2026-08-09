<!-- Placeholder legend:
     <fill with real value>           = replace with a real fact from the repo
     <entire line in angle brackets>  = delete the whole line if not applicable
     trailing <clause>                = fill in, or delete the clause if not applicable
     Do not leave <TODO> markers. Delete what does not apply. -->

# AGENTS.md

<one-sentence project description — what it does, not why>.

**Read [STYLE.md](STYLE.md) entirely before any code change.** It owns <what STYLE.md owns: blueprint placement, API contract, naming, code style>. This file owns project knowledge, commands, testing, and repo-level boundaries — not style.

## Overview

- **Entry**: `<entry point, e.g. create_app() in src/__init__.py>`; WSGI: `<wsgi.py>`.
- **Run**: `<flask dev> (all-in-one) or <flask run> / <flask run-scheduler> (split)`.
- <Scheduler/process lives in its own service (see <overlay path>), not the web process.>

## Tech Stack

- **<Language> <version>** via [<package manager>](<docs url>). Lockfile: `<lockfile>`. Install: `<install command>`. Never `<forbidden tool>`.
- **<Runtime>** via [<version manager>](<docs url>) (declared in `<manifest>`).
- **Database**: <engine> by default (`<default path>`); overridable via `<ENV var>`.
- **Frontend tooling**: <framework + version>, configured in [`<config path>`](<config path>).
- <**Timezone**: `g.tz` set by `before_request` in `<factory>`. Use `g.tz`, not `TZ`, inside views.>
- <**Container**: Dockerfile uses `<base image>` + `<install tool>` (not `<expected tool>`) — intentional for <reason>; do not "fix" without approval.>

## File Structure

- `<src/dir/>` — <one-line purpose>
- `<src/dir/>` — <one-line purpose>
- `<src/dir/>` — <one-line purpose>
- `<migrations/>` — <one-line purpose>
- `<tests/>` — <one-line purpose>

## Commands

Run from repo root. Auto-fix first, then verify what remains.

| Scope                | Auto-fix                  | Verify                  |
| -------------------- | ------------------------- | ----------------------- |
| Backend (<lang>)     | `<format command>`        | `<lint command>` then `<test command>` |
| Migrations (model)   | —                         | `<migrate command>` (review + rename) then `<upgrade command>` |
| Frontend (<lang>)   | `<format command>` and `<lint --fix command>` | `<lint check command>` |
| Frontend types/tests | —                         | `<type-check command>` / `<test command>` |
| Bundling/runtime     | —                         | `<build command>`       |

- `<package manager> run` uses the lockfile interpreter; never invoke `<forbidden>` directly.
- `<lint script>` is `<components>`; there is no `<lint:fix>` script — pass `--fix` to <linter> explicitly.
- `<migrate command>` only generates a revision; `<upgrade command>` applies, `<downgrade command>` reverts.

## Testing

- **<lang>**: <runner> <version> — [`<test dir>`](<test dir>). Fixtures in [`<conftest>`](<conftest>): <session-scoped `app` (in-memory <db>, `<JOBS=[]>`), per-test `clean_db` (truncates every table + clears cache)>.
- **<lang>**: <runner> + <env> — [`<test dir>`](<test dir>). <`$route` and `$inertia` are globals (typed by `<types file>`); `window.reverseUrl` is provided by the base template, so mocks must set it in `beforeEach`.>
- **Test DB**: `<db url>` via `<ENV var>` in <conftest>. No external DB service.
- **Property tests**: <library> available in dev extras; use for pure utilities (e.g. `<utils/file.py>`). Do not property-test code that touches the database or the network.
- CI runs <which suites> on PRs to `<branch>`.

## Boundaries

### ✅ Always
- Go through `<factory function>` for tests and CLI commands so `app.config` and extensions initialise.
- Run the relevant auto-fix + verify commands from the table above before declaring work done.
- Update [README.md](README.md), [STYLE.md](STYLE.md), or this file when adding/removing/renaming a module, blueprint, CLI command, route, scheduled job, dependency, or public API.

### ⚠️ Ask First
- Editing `<ci/workflows path>`.
- Changing the Dockerfile's `<non-default choice>` choice.
- Schema changes that are not portable to <db engine> (may need split revisions).

### 🚫 Never
- Edit `<generated/vendored dirs>`.
- Edit `<migrations/versions/*.py>` to "fix" history — write a new revision.
- Commit secrets, real credentials, or local `.env` values. <Mask secrets before persisting in the `config` table (see `<service file>:<function>`).>
- Commit `<instance/app.log>` or other runtime files under `<instance/>`.
- Bypass the application factory.
- Run the scheduler inside the web process.
- Invoke `<forbidden tool>` directly — use `<expected tool>`.
