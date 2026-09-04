# Deployment

Deployment is automated through the `Deploy` workflow (`.github/workflows/deploy.yml`)
and runs on a self-hosted runner.

## Trigger

- A GitHub **release is published** (`release` event, type `published`) on the
  repository that owns this codebase.
- The workflow runs on the `self-hosted`/`promotion` runner and deploys the
  exact released commit (tag ref checked out by SHA).

## Prerequisites

The runner needs these repository variables and configuration:

| Item              | Source / value                                  |
| ----------------- | ----------------------------------------------- |
| `DEPLOY_DIR`      | Repository variable; working directory of the app on the runner |
| `PATH_PHP`        | Repository variable; absolute path to the PHP binary |
| `PATH_COMPOSER`   | Repository variable; absolute path to the Composer binary |
| Node.js           | Read from `package.json` (`volta` pin), installed via `actions/setup-node` |
| Working tree      | Must be clean; the workflow aborts if `git status` is not empty |

## Deployment process

1. **Pre-flight** — verify PHP/Node availability and a clean working tree; record the current commit for rollback.
2. **Maintenance mode** — `php artisan down`.
3. **Checkout** — fetch and check out the released commit.
4. **Dependencies** — `composer install --no-dev --optimize-autoloader` and `npm ci`.
5. **Frontend build** — `npm run build`; requires `public/build/manifest.json`.
6. **Migrate** — `php artisan migrate --force --isolated`; the workflow tracks whether migrations were applied (needed for rollback decisions).
7. **Optimize** — `php artisan optimize:clear`, then `php artisan optimize`.
8. **Queue** — `php artisan queue:restart` to pick up new code.
9. **Health check** — `php artisan about --only=environment`.
10. **Exit maintenance** — `php artisan up` (always runs if maintenance was enabled).
11. **Deployment status** — a GitHub deployment with environment `production` is recorded.

## Rollback

If any step fails after checkout, the workflow rolls back automatically:

- `php artisan migrate:rollback --force` **only if** migrations were applied in this deploy.
- `git reset --hard <previous commit>`.
- Reinstall dependencies, rebuild frontend assets, clear/optimize caches, and bring the app back up.

## Release notes

- Database schema changes go through migrations and are applied with `--force --isolated`; test in `local`/`preview` first.
- Releases deploy only the tagged commit — never deploy uncommitted work.
- After a release, verify the health check passed and the maintenance banner is gone before announcing.