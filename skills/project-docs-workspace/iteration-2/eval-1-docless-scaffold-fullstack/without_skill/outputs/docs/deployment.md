# Deployment

Deployment is automated via GitHub Actions (`.github/workflows/deploy.yml`) and
triggered by publishing a **GitHub release**. The target is a self-hosted
Windows runner (`[self-hosted, promotion]`).

## How it works

On release publish, the workflow runs on the production server directory
(`DEPLOY_DIR` repo/org variable):

1. **Pre-flight** — checks PHP/Composer/Node availability and a clean working
   directory; records the current commit for rollback.
2. **Maintenance mode** — `php artisan down`.
3. **Checkout** the release ref.
4. **Dependencies** — `composer install --no-dev`, `npm ci`.
5. **Build** — `npm run build` (clears `public/build` first, verifies
   `manifest.json`).
6. **Migrate** — `php artisan migrate --force --isolated`; notes whether any
   migrations applied.
7. **Optimize** — `php artisan optimize:clear` then `optimize`.
8. **Queues** — `php artisan queue:restart`.
9. **Health check** — `php artisan about --only=environment`.
10. **Maintenance off** — `php artisan up`.

### Rollback

On any failure after checkout, the workflow restores the previous commit
(`git reset --hard PREVIOUS_COMMIT`), rolls back DB migrations only if they
were applied, reinstalls dependencies, rebuilds assets and takes the app back
up. It then records a GitHub deployment/status (`production`).

## Required GitHub configuration

Repository/org variables:

| Variable | Purpose |
| --- | --- |
| `DEPLOY_DIR` | Absolute path to the application directory on the runner (also the Actions working directory). |
| `PATH_PHP` | Absolute path to the PHP executable on the runner. |
| `PATH_COMPOSER` | Absolute path to the Composer executable on the runner. |

The runner must have the production `.env` in place (the workflow never writes
it) and shell access to `php`, `composer` and `npm`/`node`.

## Release checklist

1. PRs merged to `main` and CI green.
2. Create a release in GitHub with a tag — the `Deploy` workflow runs on
   `release: published`.
3. Watch the run; a `production` deployment status is created on success.