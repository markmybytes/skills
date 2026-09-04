# House730 工具庫 (House730 Toolbox)

Internal operations toolbox for the House730 real-estate platform. A full-stack
application built with **Laravel 12**, **Inertia.js** and **Vue 3** (TypeScript),
styled with **Tailwind CSS 4 / daisyUI**.

## What it does

| Module | Purpose |
| --- | --- |
| 動態廣告 (Dynamic Ads) | Extract listings matching House730 search-result (SRP) URLs and generate per-platform feed files (JSON, CSV, RSS/XML) for external ad platforms. |
| 網頁性能 (Web Vitals) | Track Core Web Vitals for configurable URLs via Google PageSpeed Insights (lab) and the CrUX History API (field). |
| SEO 競爭分析 (SEO Dashboard) | Organic-traffic trends, keyword coverage and AI-citation tracking vs. competitors, fed by Ahrefs API v3. |
| 新盤廣告競投 (Bid System) | Run time-boxed bidding campaigns for new-development ad slots, with category/package/item management and a public application page. |
| AI 圖片修圖 (AI Retouch) | Dashboard for AI image-retouch usage/effectiveness data. |
| 實驗策略 (Experiments) | A/B-test experiment strategy charts. |
| Admin | User & permission management, session management, auth logs, system log viewer. |

## Tech stack

- **Backend:** PHP 8.2+, Laravel 12
- **Frontend:** Vue 3 (Composition API, TypeScript), Inertia.js v3, Tailwind CSS 4, daisyUI 5, Chart.js, Ziggy
- **Build:** Vite 7, `laravel-vite-plugin`
- **Tests:** PHPUnit (Pest-style feature/unit tests) + Vitest (`@vue/test-utils`)
- **Code quality:** Laravel Pint, ESLint, Prettier, `vue-tsc`
- **CI/CD:** GitHub Actions (PR checks + release-based deploy to a self-hosted Windows runner)

## Documentation

- [Getting started / setup](docs/setup.md)
- [Architecture overview](docs/architecture.md)
- [Feature modules](docs/modules.md)
- [Routes & API reference](docs/api.md)
- [Database schema](docs/database.md)
- [Development workflow](docs/development.md)
- [Deployment](docs/deployment.md)

## Quick start

```bash
# PHP dependencies
composer install

# Node dependencies
npm install

# Environment
cp .env.example .env
php artisan key:generate

# Database (MySQL) — create the DB, then:
php artisan migrate --seed

# Run everything (server, queue worker, logs, vite):
composer run dev
```

See [docs/setup.md](docs/setup.md) for the full setup, required third-party
credentials and common troubleshooting.