# Documentation audit — h730-audit-ws (h730-toolbox)

Audit mode. Read-only findings against repository evidence (composer.json, package.json,
phpunit.xml, routes/, .github/workflows/, seeders, app structure). Corrected copies of the
three documents with findings are included in this folder alongside this report.

Overall: documentation is accurate and well-maintained. Five findings, all minor; no
missing documents, no stale structure.

## Findings by document (ordered by severity)

### CONTRIBUTING.md

- `CONTRIBUTING.md` Setup blockquote — "`RefreshDatabase` creates it if missing and runs migrations" is false: the trait only runs `migrate:fresh` against an existing database; `h730_toolbox_test` must pre-exist in MySQL (CI provisions it via the mysql service `MYSQL_DATABASE`). → Reword to state the database must exist before testing → (`stale`)
- `CONTRIBUTING.md` Setup blockquote — test-database facts (database name, `.env` caveat) duplicate the `AGENTS.md` Testing section; CONTRIBUTING should link to the owner, not copy operational facts. → Replace the blockquote with a link to `AGENTS.md#testing` → (`duplicate`)
- `CONTRIBUTING.md` Before Submitting checklist — "Documentation is updated if behaviour or interfaces change (README, STYLE, DEPLOYMENT)" omits `CONTRIBUTING.md` and `AGENTS.md`. → List all five skill-managed docs (README, AGENTS, CONTRIBUTING, STYLE, DEPLOYMENT) → (`gap`)

### AGENTS.md

- `AGENTS.md` Verification Order — "the runtime `ziggy.js` is not used by this project (Inertia handles routing server-side) and must not be committed" is contradicted by the code: `resources/js/app.ts` imports and registers `ZiggyVue` from the `ziggy-js` package, which provides the runtime `route()` helper. → Reword: no generated runtime route script is committed; `route()` is served by `ziggy-js` via `ZiggyVue` → (`stale`)

### README.md

- `README.md` Generated Files — "_ide_helper.php … auto-runs on `composer install` / `update`" is inaccurate: `ide-helper:generate` runs only from `post-update-cmd` (i.e. `composer update`); the Quick Start's `composer install` does not trigger it. → State it auto-runs on `composer update` → (`stale`)

### STYLE.md — no findings

Rule priority order present (Security → Correctness → Architecture → Simplicity → Local consistency);
ownership declared; no operational facts duplicated from skill-owned docs; no dead sections;
no lint-enforceable rules restated. Claims verified against code: `Inertia::flash` and
`onFlash` are used throughout controllers/pages, `Permission` enum exists, `app/Http/Resources/`
exists, session `same_site` defaults to `lax`.

### DEPLOYMENT.md — no findings

Consistent with `.github/workflows/deploy.yml`: self-hosted runner labeled `promotion`,
pwsh/Windows VM, maintenance mode + automatic rollback, repository variables
`DEPLOY_DIR`/`PATH_PHP`/`PATH_COMPOSER`, release-triggered deploy.

### CLAUDE.md — no findings

Correct `@AGENTS.md` alias per Claude Code redirect syntax; matches skill guidance.

## Verified-against-evidence (accurate, no change needed)

- README tech stack, prerequisites, Quick Start, and command tables (composer.json / package.json scripts)
- `php artisan auth:make-admin` command signature (`app/Console/Commands/MakeAdmin.php`)
- `ziggy.d.ts` committed + CI freshness check (`.github/workflows/ci.yml`)
- `_ide_helper.php` gitignored (`.gitignore`)
- AGENTS routes/architecture summary (routes/web.php, routes/api.php); test facts (phpunit.xml, `resources/js/__tests__/setup.ts`); CI on PRs to `main`
- README seeded-data limits (DynamicAdSeeder creates rows only; WebVitalResultSeeder seeds 30 days of demo history)

## Changes applied to corrected copies (this folder)

- `README.md` — line "auto-runs on `composer install` / `update`" → "auto-runs on `composer update` (via `post-update-cmd`)"
- `AGENTS.md` — replaced the stale runtime-ziggy rationale with the accurate `ziggy-js`/`ZiggyVue` explanation
- `CONTRIBUTING.md` — corrected the `RefreshDatabase` claim; replaced duplicated test-database facts with a link to `AGENTS.md#testing`; added AGENTS + CONTRIBUTING to the docs-update checklist

## Items needing user-provided facts

- `LICENSE` — `composer.json` declares `"license": "MIT"` but no `LICENSE` file exists in the repo. Add the MIT license file, or correct the composer metadata. (Internal-tool context: decide if licensing applies at all.)
- `DEPLOYMENT.md` release process — sequential tag numbering (e.g. `v35` → `v36`) and infrastructure specifics (IIS, Nginx reverse proxy, Task Scheduler entries) are unverifiable without git history/VM access; confirm they are current.
- `CONTRIBUTING.md` conventions — Jira-key branch/PR naming (`IT-xxx`) is project policy; confirm it still holds (no evidence in repo to verify against).