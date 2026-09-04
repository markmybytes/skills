# Architecture

## Overview

Monolith rendered server-side by Laravel with a client-side Vue 3 SPA mounted
through **Inertia.js**. All internal pages live behind `/internal/*` (auth
required). A small public surface (`/newestate/*`, `/rss/*`) serves the bid
application page and generated listing feeds.

```
┌────────────────────────────────────────────┐
│                Browser                      │
│  Vue 3 SPA (Inertia) + daisyUI/Tailwind     │
└───────────────┬────────────────────────────┘
                │ Inertia (XHR + JSON page props)
┌───────────────▼────────────────────────────┐
│              Laravel 12                     │
│  routes/web.php · routes/api.php            │
│  Controllers (App\Http\Controllers)         │
│  Services (App\Services)                    │
│  Models (App\Models)                        │
└───┬────────┬──────────┬──────────┬──────────┘
    │        │          │          │
 MySQL  House730 API  Google     Ahrefs
 (own DB)  (property   (CrUX,    (SEO v3)
           queries)    PSI, OAuth)
```

## Directory layout

```
app/
├── Console/Commands/       # artisan commands (auth:make-admin, ...)
├── Enums/                  # Permission enum (authorization surface)
├── Events/                 # LoginRejected
├── Exceptions/             # SsoRejected
├── Facades/                # AppConfig facade
├── Http/Controllers/       # web + api controllers (Inertia pages)
│   ├── Api/                # JSON API endpoints
│   └── Auth/               # login / google / dev login
├── Http/Middleware/        # EnsureVisitorId, HandleInertiaRequests
├── Http/Resources/         # API resources for the bid system
├── Jobs/                   # queue jobs (dynamic-ad generation, web-vitals fetches)
├── Listeners/              # AuthEventSubscriber (auth logging)
├── Mail/                   # BidReceivedNotification
├── Models/                 # Eloquent models
├── Providers/
├── Services/               # domain logic (see modules)
└── Supports/               # DbTime (Asia/Hong_Kong now())
config/                     # Laravel config (services.* holds 3rd-party creds)
database/
├── factories/  migrations/  seeders/  schema/mysql-schema.sql
resources/js/
├── Components/             # shared Vue components (Chart, PaginationBar, ...)
├── composables/            # useAuth, useClipboard, useUniqueClipboard
├── Layouts/                # AppLayout, BiddingLayout
├── Pages/                  # Inertia pages (one per route)
└── utils/                  # index.ts, webVitalsThresholds.ts
routes/
├── web.php                 # browser routes (Inertia)
├── api.php                 # authenticated JSON API
└── console.php             # scheduled tasks + artisan commands
tests/
├── Feature/  Unit/         # PHPUnit suites
resources/js/__tests__/     # Vitest suites (components, pages, unit)
```

## Request flow

1. Browser hits a route in `routes/web.php`.
2. Controller builds data and returns an Inertia response (Vue page + props).
3. `HandleInertiaRequests` middleware injects shared props (user, permissions, env).
4. Vue renders the page inside `AppLayout`; mutations go back via Inertia POSTs.

## Authorization

- **Permission-based.** `app/Enums/Permission.php` lists every permission
  (`<module>.<action>`, e.g. `web-vitals.view`).
- Routes are guarded with the `can:` middleware (e.g.
  `->middleware('can:web-vitals.view')`).
- `App\Models\User` stores `permissions` as a JSON column and exposes
  `hasPermission()` / `permissionList()`. Admins (`is_admin`) bypass via
  `Gate::before`.
- Frontend menu visibility uses `useAuth().can()` from the shared props.

## Configuration at runtime

`AppConfiguration` model + `AppConfig` facade provide DB-backed config values
(namespace/key/value) — used for things like bid email notification settings.
See `AppConfigService`.

## Scheduling (`routes/console.php`)

| Schedule | Job |
| --- | --- |
| Daily 12:00 | Regenerate files for auto-update dynamic ads (`MakeDynamicAdFiles`). |
| Daily 21:00 | Fetch PageSpeed Insights for enabled URLs (`FetchWebVitalsPagespeed`, mobile + desktop). |
| Every other day | Fetch CrUX field data for enabled URLs (`FetchWebVitalsCrux`, PHONE + DESKTOP). |
| Monthly | Purge dynamic ads trashed > 30 days and delete their stored files. |

## Queue

`QUEUE_CONNECTION=database`. Queues used: `default`, `dynamic-ad` (ad file
generation), `notification`. Dev command `composer run dev` listens on all
three.