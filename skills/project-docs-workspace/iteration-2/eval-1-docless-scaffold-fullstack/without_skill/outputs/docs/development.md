# Development

## Commands

| Command | Purpose |
| --- | --- |
| `composer run dev` | Run server + queue worker + pail + Vite concurrently. |
| `npm run dev` | Vite dev server. |
| `npm run build` | Production frontend build (`run-p type-check "build-only"`). |
| `npm run type-check` | `vue-tsc --build` type check. |
| `npm run lint` / `lint:fix` | ESLint on `resources`. |
| `npm run format` / `format:check` | Prettier (check mode used in CI). |
| `npm run test` | Vitest suite (`vitest run`). |
| `composer run test` | PHPUnit suite (clears config first). |
| `vendor/bin/pint` | Laravel Pint (run `pint --test` to check). |
| `php artisan ziggy:generate --types-only` | Regenerate Ziggy route types after route changes. |

## Code style

- **PHP:** Laravel Pint (default preset). CI runs `vendor/bin/pint --test`.
- **JS/TS/Vue:** ESLint + Prettier with the Vue flat config and
  `prettier-plugin-tailwindcss`. CI runs `npm run lint` and `npm run format:check`.
- **TypeScript:** strict via `vue-tsc`. CI runs `npm run type-check`.

## Testing

### PHP (PHPUnit)

- `tests/Unit` — models, services, jobs.
- `tests/Feature` — controllers, auth, authorization middleware, services,
  session handling, route availability, seeding.
- Test DB is MySQL `h730_toolbox_test` (see `phpunit.xml`; CI provisions a
  MySQL 8.4 service). `QUEUE_CONNECTION=sync` and `CACHE_STORE=array` under test.

### JS (Vitest)

- `resources/js/__tests__/` — component specs (`.spec.ts`) under
  `components/`, `pages/`, `unit/`; jsdom environment with `@vue/test-utils`
  and the shared `setup.ts`.

## Contribution workflow

1. Branch off `main`.
2. Make changes; add/extend tests where behaviour changes.
3. Run the same checks as CI:
   ```bash
   vendor/bin/pint --test
   npm run lint
   npm run format:check
   npm run type-check
   ```
4. If routes changed, regenerate `resources/js/ziggy.d.ts` and commit it:
   ```bash
   php artisan ziggy:generate --types-only
   ```
5. Run both test suites:
   ```bash
   npm run test
   composer run test
   ```

## CI

`.github/workflows/ci.yml` runs on PRs to `main`:

1. **Lint & Type Checks** — PHP 8.4 + Node: Pint, ESLint, Prettier check,
   `vue-tsc`, Ziggy types staleness.
2. **Full Tests** — same deps against a MySQL 8.4 service: Vitest, frontend
   build, PHPUnit.

## Gotchas

- **MySQL JSON defaults** — MySQL doesn't support default values on JSON
  columns; the default permission set is declared on the `User` model
  (`$attributes['permissions']`). Update that array when new dashboards are added.
- **Ziggy staleness** — CI fails if `resources/js/ziggy.d.ts` is out of date
  with the routes; regenerate after editing route definitions.
- **Seeders are env-gated** — they no-op outside `local`/`preview`.
- **Dev login only in `local`/`testing`** — routes are registered conditionally
  on the environment.