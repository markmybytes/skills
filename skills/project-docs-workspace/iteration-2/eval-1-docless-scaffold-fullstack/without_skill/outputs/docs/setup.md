# Setup

## Requirements

- PHP >= 8.2 (CI uses 8.4)
- Composer
- Node.js (see `volta` pin in `package.json` — Node 24.x)
- MySQL 8.x

## Installation

```bash
composer install
npm install
```

## Environment

Copy the example environment and generate an app key:

```bash
cp .env.example .env
php artisan key:generate
```

### Required environment variables

Everything above the "Project-specific" separator in `.env.example` is standard
Laravel configuration. Project-specific variables:

| Variable | Required | Purpose |
| --- | --- | --- |
| `HOUSE730_API_HOST` | yes | House730 API host for property queries. |
| `HOUSE730_APP_KEY` | yes | API auth key for House730 API (see HOUSE-174). |
| `HOUSE730_ENCRYPTION_KEY` | yes | Encryption key for House730 API auth. |
| `HOUSE730_DB_HOST` / `_PORT` / `_DATABASE` / `_USERNAME` / `_PASSWORD` | for production | Read-only connection to the main House730 production DB for property queries. |
| `GOOGLE_API_KEY` | for Web Vitals | Google API key for PageSpeed Insights and CrUX History API. |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | for SSO | Google OAuth application credentials. |
| `GOOGLE_REDIRECT_URI` | for SSO | Must match the OAuth consent screen redirect. |
| `GOOGLE_OAUTH_DOMAIN` | for SSO | Allowed email domain for Google login. Logins from other domains are rejected. |
| `AHREFS_API_KEY` | for SEO | Ahrefs API v3 Bearer token. |
| `AHREFS_SITE_AUDIT_PROJECT_ID` | for SEO | Ahrefs project id for site audit. |
| `AHREFS_GSC_PROJECT_ID` | for SEO | Ahrefs project id for GSC data. |

## Database

Default connection is MySQL (`DB_CONNECTION=mysql`). After creating the database:

```bash
php artisan migrate
```

Seeders only run in `local`/`preview` environments (`database/seeders/DatabaseSeeder.php`).

A full MySQL schema dump lives at `database/schema/mysql-schema.sql`.

## Authentication

Login is Google SSO only:

1. `GET /auth/google/redirect` — start the OAuth flow.
2. Callback provisions the user via `UserProvisioningService` (email domain must match `GOOGLE_OAUTH_DOMAIN`).
3. Users without any permissions land on an empty dashboard — contact an admin to grant permissions or run:

```bash
php artisan auth:make-admin user@yourdomain.com
```

In `local` and `testing` environments only, a dev login bypass is available:

- `GET /auth/dev` — pick any user.
- `POST /auth/dev/login`.

## Running locally

```bash
composer run dev
```

This runs (via `concurrently`):

- `php artisan serve` (server on `http://localhost:8000`)
- `php artisan queue:listen --queue=default,dynamic-ad,notification` (queue worker; dynamic-ad generation needs this)
- `php artisan pail` (log tail)
- `npm run dev` (Vite dev server)

## Troubleshooting

| Symptom | Cause / fix |
| --- | --- |
| Google login rejects "非授權的網域" | `GOOGLE_OAUTH_DOMAIN` mismatch with the email domain. |
| Login throws "GOOGLE_OAUTH_DOMAIN is not configured" | Missing env var — the service throws `LogicException`. |
| Dynamic-ad files not generated | Queue worker not running (`dynamic-ad` queue). |
| RSS feed empty | SRP filters match no listings; relax the search conditions. |
| Type-check fails on `resources/js/ziggy.d.ts` | Regenerate Ziggy route types: `php artisan ziggy:generate --types-only` (CI enforces this). |