# Documentation Audit Report — h730-toolbox

Audited: `README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `DEPLOYMENT.md`, `STYLE.md`, `.env.example`, `.agents/skills/style-review/SKILL.md`, `.github/workflows/*.yml`, and in-app Guide pages (`resources/js/Pages/Guide/*.vue`). All claims were cross-checked against `composer.json`, `package.json`, `phpunit.xml`, `routes/*.php`, `config/*`, `bootstrap/app.php`, seeders, and controllers.

Overall the documentation is in good shape and highly consistent with the code. The following issues were found.

---

## 1. Inaccurate / wrong statements

### 1.1 README.md (line 83) — `_ide_helper.php` does not auto-run on `composer install`

> "Gitignored; auto-runs on `composer install` / `update`."

`composer.json` runs `@php artisan ide-helper:generate` only in the `post-update-cmd` hook. Per Composer semantics, `post-update-cmd` fires on `composer update`, and on `composer install` **only when no lock file is present**. This repo commits `composer.lock`, so a normal `composer install` will **not** regenerate `_ide_helper.php`. There is no `post-install-cmd` hook defined.

Fix: say "auto-runs on `composer update` (via the `post-update-cmd` hook)", or add a `post-install-cmd` hook.

**Action taken: README.md corrected (copy in `outputs/`).**

### 1.2 AGENTS.md (line 31) — Ziggy runtime claim is wrong/misleading

> "Always pass `--types-only` — the runtime `ziggy.js` is not used by this project (Inertia handles routing server-side) and must not be committed."

The Ziggy runtime **is** used: `resources/js/app.ts` imports `ZiggyVue` from `ziggy-js` (a `package.json` dependency), and `resources/views/app.blade.php` renders the `@routes` directive to inject the route list. What is unused is only the **generated** `resources/js/ziggy.js` file; the stated reason ("Inertia handles routing server-side") is incorrect.

Fix: reword to "routes are injected at runtime via the `@routes` Blade directive and the `ziggy-js` package, so the generated `resources/js/ziggy.js` is not used and must not be committed."

**Action taken: AGENTS.md corrected (copy in `outputs/`).**

### 1.3 `.agents/skills/style-review/SKILL.md` (line 46) — references a non-existent "verification section of `STYLE.md`"

> "Conclude by reading the verification section of `STYLE.md` and reminding the user to run the exact validation commands..."

`STYLE.md` has **no** "Verification" section — it ends at "API Response Contract". The verification/validation commands actually live in `README.md` (§ "Quality & Testing") and `AGENTS.md` (§ "Verification Order"). Following the instruction literally would fail.

Fix: point to `README.md` § Quality & Testing / `AGENTS.md` § Verification Order.

**Action taken: SKILL.md corrected (copy in `outputs/`).**

### 1.4 `.env.example` (line 1) — `APP_NAME=Laravel` (minor)

The app is named `h730-toolbox` (README title). `config('app.name')` drives the default browser `<title>` fallback (`resources/views/app.blade.php`) and the mail from-name. A fresh local setup would show "Laravel" in the title. Leave as-is or set `APP_NAME=h730-toolbox`. Not changed in the copies (arguably a config default, not a doc statement), but worth fixing.

---

## 2. Duplicated content

### 2.1 Verification / fix command table duplicated verbatim

`README.md` (§ "Quality & Testing", lines 62–66) and `AGENTS.md` (§ "Verification Order", lines 25–29) carry the same auto-fix/check table (pint / composer test / format+lint:fix / type-check / build). Duplicating the table in two files invites drift when commands change. Recommendation: keep one canonical table (README) and have AGENTS.md link to it.

### 2.2 Pagination rule duplicated

The sentence "return paginator/API Resource collection directly — Laravel auto-injects `links`/`meta`" appears in both `AGENTS.md` (Architecture, line 41) and `STYLE.md` (API Response Contract §3, line 148). STYLE.md is the canonical source per AGENTS.md's own doc map; the AGENTS.md copy could be trimmed to a pointer.

### 2.3 Testing facts repeated across three docs

PHPUnit vs Vitest layout, the `h730_toolbox_test` MySQL requirement, `route()` mocking, and "read STYLE.md first" are each repeated in `README.md`, `AGENTS.md`, and `CONTRIBUTING.md`. Acceptable for onboarding redundancy, but a candidate for consolidation.

No files changed for 2.1–2.3 — removal/consolidation is a maintainer choice; the exact duplicate lines are listed here so they can be deduped.

---

## 3. Missing documentation

### 3.1 `php artisan dynamic-ad:restore` command is undocumented

`app/Console/Commands/RestoreDynamicAds.php` (`dynamic-ad:restore {dynamicAd?} --all`) exists but appears nowhere in README/AGENTS/STYLE/DEPLOYMENT. README's "Standalone Commands" table documents `auth:make-admin` but omits this command. Fix: add it to the README commands table.

**Action taken: row added to README.md (copy in `outputs/`).**

### 3.2 Scheduled tasks defined in `routes/console.php` are not documented

The scheduler defines four recurring jobs (monthly purge of soft-deleted dynamic ads, daily 12:00 auto-update of dynamic-ad files, daily 21:00 PageSpeed fetch, odd-day CrUX fetch) — none described anywhere in the repo docs. `DEPLOYMENT.md` mentions Task Scheduler entries exist but does not describe the schedules. Low priority (ops-owned), but the schedule could be recorded in DEPLOYMENT.md for reference. Not changed.

### 3.3 (Minor) `/rss-filters/{any?}` redirect route

`routes/web.php` line 135 registers a `GET /rss-filters/*` → `dynamic-ads.index` redirect that is not mentioned in any doc. Trivial, no change made.

---

## 4. Verified correct (no action)

- Tech stack claims (Laravel 12, Inertia v3, Vue 3 + TS, Tailwind v4 + DaisyUI 5, MySQL 8 + `h730` secondary connection) — match `composer.json`, `package.json`, `config/database.php`.
- `composer dev` description and queue flags — match `composer.json` `dev` script.
- `auth:make-admin`, ziggy `--types-only`, `_ide_helper.php` gitignore, CI freshness check for `ziggy.d.ts` — match code and `.github/workflows/ci.yml`.
- Seeded-data limits (dynamic-ads seed DB rows only; Web Vitals demo history; `GOOGLE_API_KEY` needed for live jobs) — match seeders.
- `h730_toolbox_test` MySQL requirement for PHPUnit — matches `phpunit.xml` and CI service.
- STYLE.md permission model, route chain order, `->scoped()` nesting, API envelope, flash conventions, SameSite=Lax CSRF note — all match `routes/*.php`, `bootstrap/app.php`, controllers, and `config/session.php`.
- DEPLOYMENT.md infrastructure, `promotion` label, `DEPLOY_DIR`/`PATH_PHP`/`PATH_COMPOSER` vars, rollback procedure — match `.github/workflows/deploy.yml`.
- Guide pages (`BidSystem.vue`, `DynamicAdGuide.vue`, `CruxHistoryGuide.vue`) — tier sorting, default tiers, `/rent/`/`/buy/` URL requirement, category edit/delete restrictions all consistent with controllers/services.

---

## Files copied to outputs (with changes)

| File | Change |
| --- | --- |
| `README.md` | 1) `_ide_helper.php` note: "auto-runs on `composer install` / `update`" → "auto-runs on `composer update` (via the `post-update-cmd` hook)". 2) Added `php artisan dynamic-ad:restore {id} / --all` row to Standalone Commands. |
| `AGENTS.md` | Reworded the Ziggy `--types-only` note: runtime routes come from `@routes` + `ziggy-js`; the generated `resources/js/ziggy.js` is unused and must not be committed. |
| `.agents/skills/style-review/SKILL.md` | Verification-section reference fixed to point at README.md § Quality & Testing / AGENTS.md § Verification Order (STYLE.md has no verification section). |