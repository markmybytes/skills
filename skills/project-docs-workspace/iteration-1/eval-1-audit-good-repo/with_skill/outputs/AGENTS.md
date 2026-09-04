# AGENTS.md

Agent boot/invariant guidance for this Laravel 12 + Vue 3 (Inertia SPA) internal tool. Domain rules live in the linked docs below — read them before working in their area.

## Documentation map

| Doc                                | Purpose / ownership                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------- |
| [README.md](README.md)             | Onboarding, prerequisites, commands, quality/testing overview                                 |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution workflow; branch/commit/PR/Jira naming and Git workflow                          |
| [STYLE.md](STYLE.md)               | Coding style, architecture, permissions, API contract; **mandatory read before code changes** |
| [DEPLOYMENT.md](DEPLOYMENT.md)     | Release/deployment/ops/recovery                                                               |

## Working rules

- Read [STYLE.md](STYLE.md) entirely before any code changes.
- Follow [CONTRIBUTING.md](CONTRIBUTING.md) for Git workflow, Jira traceability, and merge strategy.
- Read [DEPLOYMENT.md](DEPLOYMENT.md) before release/ops work.
- Update the owning doc (README, CONTRIBUTING, STYLE, or DEPLOYMENT as applicable) when you add, remove, or rename a module, route, command, dependency, or public API.

## Verification Order

Auto-fix first, then check for what remains.

| Scope             | Fix                                  | Check                |
| ----------------- | ------------------------------------ | -------------------- |
| Backend (PHP)     | `./vendor/bin/pint`                  | `composer test`      |
| Frontend (Vue/TS) | `npm run format && npm run lint:fix` | `npm run type-check` |
| Bundling/runtime  | —                                    | `npm run build`      |

After changing routes, regenerate types with `php artisan ziggy:generate --types-only`. Always pass `--types-only` — no runtime route script is generated or committed; the runtime `route()` helper comes from the `ziggy-js` package via `ZiggyVue` in `resources/js/app.ts`. CI checks `resources/js/ziggy.d.ts` freshness.

## Architecture

[STYLE.md](STYLE.md) is canonical for structure and conventions; summary below.

- **Web controllers** → `app/Http/Controllers/` — return Inertia pages
- **API controllers** → `app/Http/Controllers/Api/` — return JSON (envelope contract)
- **Services** → `app/Services/`, **Jobs** → `app/Jobs/`, **Supports** → `app/Supports/`
- **Routes**: public at `/newestate` (bid submissions), `/rss/{rss}`; internal under `/internal/` with `auth`; API under `/api/` with `auth`
- **Pagination**: return paginator/API Resource collection directly — Laravel auto-injects `links`/`meta`

## Testing

Details in [CONTRIBUTING.md](CONTRIBUTING.md) / [README.md](README.md); key facts:

- **PHP**: PHPUnit 11 — `tests/Unit/` + `tests/Feature/`. **Requires a running MySQL** with database `h730_toolbox_test` (defined in `phpunit.xml`). `composer test` will fail without it. `composer test` clears config first.
- **JS**: Vitest with jsdom — `resources/js/__tests__/`. Globals `route()` mocked in setup.
- CI runs both test suites on PRs to `main`.

## Constraints

- Do not edit `.github/workflows/**` without explicit instruction
- Do not edit `vendor/**`, `node_modules/**`, or `public/build/**`
- Do not edit `database/schema/mysql-schema.sql` unless schema bootstrap is the task scope
- `_ide_helper.php` is auto-generated — do not manually edit
- Do not commit secrets or local `.env` values
- Do not remove or bypass access control middleware without explicit approval
- Ask for clarification when requirements are ambiguous.

## Environment

- Node via Volta (declared in `package.json`)
- Tailwind v4 + DaisyUI 5 configured inline in `resources/css/app.css` — no `tailwind.config.*`
- `.env.example` defaults to MySQL (configure DB for your instance before booting)