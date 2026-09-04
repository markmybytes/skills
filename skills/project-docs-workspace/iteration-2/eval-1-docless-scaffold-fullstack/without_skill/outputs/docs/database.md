# Database

MySQL (default `DB_CONNECTION=mysql`). A schema dump is maintained at
`database/schema/mysql-schema.sql`. New tables are defined in
`database/migrations/`.

## Tables

### Framework tables

| Table | Purpose |
| --- | --- |
| `users` | Accounts, `google_id`, `is_admin`, JSON `permissions`, soft deletes. |
| `sessions` | Laravel session store (DB driver). |
| `cache` / `cache_locks` | Cache store. |
| `jobs` / `job_batches` / `failed_jobs` | Queue. |
| `migrations` | Migration bookkeeping. |
| `auth_logs` | Auth events (login / logout / rejected) with email, IP, user agent. |

### Feature tables

#### Dynamic ads

| Table | Columns (highlights) | Notes |
| --- | --- | --- |
| `dynamic_ads` | `title`, `srp_urls` (json), `company_ids` (json), `exclude_property_ids` (json), `exclude_estate_ids` (json), `top_types` (json), `has_image`, `is_exclude_batch`, `is_auto_update`, `day_before_expiry`, `accessed_at`, soft deletes | One row per ad filter. |
| `dynamic_ad_files` | `uuid`, `format` (json/csv/xml), `for` (platform), FK `dynamic_ad_id` (cascade) | Generated feed files on the `local` disk under `dynamic-ads/`. |

#### Web vitals

| Table | Columns (highlights) | Notes |
| --- | --- | --- |
| `web_vital_urls` | `url` (unique), `fetch_enabled` | Tracked URLs. |
| `web_vital_pagespeeds` | FK `web_vital_url_id`, `form_factor` enum(`phone`,`desktop`), `raw_data` (json), `created_at` | Lab data; indexed `(url_id, form_factor, created_at)`. |
| `web_vital_crux_results` | FK `web_vital_url_id`, `form_factor` enum(`phone`,`desktop`,`tablet`), `raw_data` (json), `created_at` | Field data; indexed `(url_id, form_factor, created_at)`. |

#### Bid system

| Table | Columns (highlights) | Notes |
| --- | --- | --- |
| `bid_categories` | `parent_id` (self FK, nullable, null-on-delete), `name` | Two-level tree. Immutable once used in campaigns. |
| `bid_campaigns` | `application_start_at`, `application_end_at`, `start_date`, `end_date`, `is_active` | One active campaign at a time; soft deletes. |
| `bid_packages` | FK `campaign_id`, FK `category_id`, `tiers` (json) | Tiers auto-sorted by price; highest = buy-out. Restrict-on-delete FKs. |
| `bid_items` | FK `package_id`, `name`, `is_visible` | Cascade on package delete. |
| `bid_submissions` | FK `package_id`, agent contact fields, `chosen_level`, `amount_paid`, `client_ip`, `created_at` (ms precision) | Restrict-on-delete; soft deletes added later. |

#### Config

| Table | Columns | Notes |
| --- | --- | --- |
| `app_configurations` | `namespace`, `key`, `value` | Runtime config, unique (namespace, key). Accessed via `AppConfig` facade. |

## Foreign-key behavior notes

- `dynamic_ad_files.dynamic_ad_id` → `ON DELETE CASCADE`.
- `bid_packages` → campaigns and categories: `ON DELETE RESTRICT` (protects
  historical data — categories cannot be deleted once referenced).
- `bid_items.package_id` → `ON DELETE CASCADE`.
- `bid_submissions.package_id` → `ON DELETE RESTRICT`.

## Seeding

`DatabaseSeeder` runs only in `local` / `preview` environments and seeds
users, dynamic ads (+files), web-vital URLs (+results), bid categories,
campaigns, packages, items and submissions for development.