# Routes & API reference

Two surfaces: **internal** (authenticated, permission-gated, Inertia pages +
JSON) and **public** (bid application page + generated feeds).

## Public routes

| Method | Path | Name | Notes |
| --- | --- | --- | --- |
| GET | `/` | `bid.index` | Public bid landing (`/newestate`). |
| GET | `/{category}` | `public.bid.show` | Public bid category page. |
| POST | `/packages/{package}/submissions` | `packages.submissions.store` | Submit a bid application. |
| GET | `/rss/{rss}` | `rss.show` | Serve a generated dynamic-ad feed file. |
| GET | `/rss-filters/{any?}` | — | Legacy redirect → `dynamic-ads.index`. |

## Auth routes

| Method | Path | Name |
| --- | --- | --- |
| GET | `/auth/login` | `login` |
| POST | `/auth/logout` | `logout` |
| GET | `/auth/google/redirect` | `auth.google.redirect` |
| GET | `/auth/google/callback` | `auth.google.callback` |
| GET | `/auth/dev` | `auth.dev.index` (local/testing only) |
| POST | `/auth/dev/login` | `auth.dev.login` (local/testing only) |

## Internal routes (auth required, `auth` middleware)

All under `/internal`. Permissions shown in parentheses.

| Method | Path | Name | Permission |
| --- | --- | --- | --- |
| GET | `/internal` | `internal.index` | any authenticated user |
| GET | `/internal/logs` / DELETE | `logs.index` / `logs.destroy` | `logs.view` |
| GET | `/internal/guide/dynamic-ad` · `crux-history` · `bid-system` | `guide.*` | any authenticated user |
| Resource | `/internal/dynamic-ads` | `dynamic-ads.*` | `dynamic-ads.view` |
| GET | `/internal/web-vitals` | `web-vitals.index` | `web-vitals.view` |
| GET | `/internal/web-vitals/history` | `web-vitals.history` | `web-vitals.view` |
| Resource (index/store/destroy) | `/internal/web-vitals/urls` | `web-vitals.urls.*` | `web-vitals.view` |
| PATCH | `/internal/web-vitals/urls/{url}/toggle` | `web-vitals.urls.toggle` | `web-vitals.view` |
| GET | `/internal/web-vitals/urls/{url}/crux` · `field` · `lab` | `web-vitals.urls.{crux,field,lab}` | `web-vitals.view` |
| GET | `/internal/ai-retouch/dashboard` | `ai-retouch.dashboard` | `ai-retouch.view` |
| GET | `/internal/ai-retouch/images` | `ai-retouch.images` | `ai-retouch.view` |
| Resource | `/internal/bid/campaigns` (except show) | `bid.campaigns.*` | `bid-campaigns.view` |
| GET | `/internal/bid/campaigns/{campaign}/submissions` | `bid.campaigns.submissions` | `bid-campaigns.view` |
| POST | `/internal/bid/campaigns/{campaign}/clone` | `bid.campaigns.clone` | `bid-campaigns.view` |
| GET/DELETE | `/internal/bid/campaigns/{campaign}/submissions` · `/{submission}` | `bid.campaigns.submissions.*` | `bid-campaigns.view` |
| POST | `/internal/bid/campaigns/{campaign}/submissions/{submission}/restore` | `bid.campaigns.submissions.restore` | `bid-campaigns.view` |
| Resource (index/create/store/update) | `/internal/bid/categories` | `bid.categories.*` | `bid-campaigns.view` |
| Resource (edit/update) | `/internal/bid/campaigns/{campaign}/packages` | `bid.campaigns.packages.*` | `bid-campaigns.view` |
| POST | `/internal/bid/campaigns/{campaign}/packages/sync` | `bid.campaigns.packages.sync` | `bid-campaigns.view` |
| Resource (store/destroy) | `/internal/bid/campaigns/{campaign}/packages/{package}/items` | `bid.campaigns.packages.items.*` | `bid-campaigns.view` |
| DELETE | .../`items` (all) | `bid.campaigns.packages.items.destroyAll` | `bid-campaigns.view` |
| Resource (index/store) | `/internal/bid/configurations` | `bid.configurations.*` | `bid-campaigns.view` |
| GET/DELETE | `/internal/sessions` · `/{id}` | `sessions.index` / `sessions.destroy` | `sessions.view` |
| Resource (except create/show, withTrashed edit/update) | `/internal/users` | `users.*` | `users.view` |
| POST | `/internal/users/{user}/restore` | `users.restore` | `users.view` |
| GET | `/internal/auth-logs` | `auth-logs.index` | `auth-logs.view` |
| GET | `/internal/experiment` · `/{name}` | `experiment.index` / `experiment.show` | `experiment.view` |
| GET | `/internal/seo` | `seo.dashboard` | `seo.view` |

## API routes (`/internal/api`, auth + permission)

| Method | Path | Name | Permission |
| --- | --- | --- | --- |
| POST | `/internal/api/crux-history` | `api.crux-history.query` | `web-vitals.view` |
| GET | `/internal/api/dynamic-ads/queue-status` | `api.dynamic-ads.queue-status` | `dynamic-ads.view` |
| POST | `/internal/api/dynamic-ads/preview` | `api.dynamic-ads.preview` | `dynamic-ads.view` |
| PATCH | `/internal/api/dynamic-ads/{id}/status` | `api.dynamic-ads.update-status` | `dynamic-ads.view` |

## Artisan commands

| Command | Purpose |
| --- | --- |
| `auth:make-admin {email}` | Promote a user to admin. |
| `ziggy:generate --types-only` | Regenerate `resources/js/ziggy.d.ts` (CI checks staleness). |

## Scheduled tasks

See `routes/console.php` — summarized in [architecture.md](architecture.md#scheduling-routesconsolephp).