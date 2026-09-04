# Feature modules

## Dynamic Ads (動態廣告)

Turn House730 search-result URLs into product feeds for external ad platforms.

**Flow:** an admin creates a "dynamic ad" filter from one or more SRP URLs
(containing `/rent/` or `/buy/`), plus optional filters (company ids, excluded
property/estate ids, listing top-types, image-only, exclude batch listings,
expiry window). The `MakeDynamicAdFiles` queue job (`dynamic-ad` queue):

1. Parses each SRP URL with `SearchPayloadBuilder` (location/price/area/age/type
   tokens in the URL path + query string).
2. Fetches listings from the House730 API via `ListingFetcher`.
3. Deduplicates by `propertyID` and applies exclude filters via
   `DynamicAdListingFilter`.
4. Writes four feed formats per ad via `RssGenerator`:
   - `base` — JSON array (canonical, used as input for the others)
   - `google` — Google Ads CSV feed
   - `meta` — Google Shopping RSS/XML feed
   - `appier` — Appier CSV feed

Files are stored on the `local` disk under `dynamic-ads/{filename}` and served
via `GET /rss/{rss}`. Optional `is_auto_update` regenerates feeds daily at
12:00. Trashed ads are purged (files + rows) after 30 days.

**Routes:** `/internal/dynamic-ads` (CRUD, `dynamic-ads.view`), `/rss/{rss}` (public),
`/internal/api/dynamic-ads/*` (queue status, preview, status updates).
**Guide page:** `/internal/guide/dynamic-ad`.

## Bid System (新盤廣告競投)

Time-boxed bidding for new-development advertising slots.

- **Campaign** — application open/close times + ad display window. One active
  campaign at a time. Campaigns can be cloned (including packages).
- **Category** — two-level tree: master categories group sub-categories; only
  sub-categories carry bid packages. Categories are immutable once used.
- **Package** — per (campaign, category), a set of price tiers (L1–L3). Tiers
  are auto-sorted; the highest tier is the "buy-out" price.
- **Item** — the actual new-development listings shown on the public page. A
  category shows only when it has at least one visible item.
- **Submission** — agent applications (company, agent, phone, license, chosen
  tier, amount, client IP). Soft-deletable/restorable.
- **Config** — email notifications on submission (`AppConfig` facade +
  `BidReceivedNotification` mail).

Public page at `/newestate` (`bid.index` / `public.bid.show`) shows packages in
states `upcoming` / `open` / `closed` (bought out or past deadline).
Submissions posted at `/newestate/packages/{package}/submissions`.

**Routes:** `/internal/bid/*` (campaigns, categories, packages, items,
configurations; `bid-campaigns.view`). **Guide:** `/internal/guide/bid-system`.

## Web Vitals (網頁性能)

Track Core Web Vitals per configured URL, two data sources:

- **Lab — PageSpeed Insights** (`FetchWebVitalsPagespeed`, `PageSpeedProvider`):
  mobile + desktop snapshots, stored in `web_vital_pagespeeds`.
- **Field — CrUX History API** (`FetchWebVitalsCrux`, `CruxProvider`): 28-day
  rolling windows, P75 percentile, PHONE + DESKTOP form factors, stored in
  `web_vital_crux_results`.

URLs are managed at `/internal/web-vitals/urls` (`web-vitals.view`), each with a
`fetch_enabled` toggle. Per-URL views: CrUX history, field metrics, lab metrics.
An authenticated JSON endpoint (`POST /internal/api/crux-history`) serves ad-hoc
CrUX queries. Thresholds live in `resources/js/utils/webVitalsThresholds.ts`.

**Routes:** `/internal/web-vitals/*`. **Guide:** `/internal/guide/crux-history`.

## SEO Dashboard (SEO 競爭分析)

Competitive analysis fed by the **Ahrefs API v3** (`AhrefsProvider`), composed by
`SeoDashboardService` and rendered on `/internal/seo` (`seo.view`):

- Traffic history & share-of-voice vs. competitor domains (`DomainCatalog`).
- Keyword traffic share (top 3 / 4–10 / 11–50).
- Competitive trends (top-3/top-10 keyword coverage, domain rating).
- AI citations across ChatGPT / Gemini / Perplexity / Google AIO / Copilot / Grok.
- Technical: site audit, issues, GSC performance (clicks, position, devices).
- Rank-Tracker cohort program tracking (manually captured weekly snapshots — see
  `buildCohorts()` in `SeoDashboardService`).

Failures per API call degrade gracefully (logged, section shows "unavailable").

## Auth, Users & Admin

- **Google SSO** via Laravel Socialite. `UserProvisioningService` validates
  domain, provisions/updates the user, rejects deactivated accounts
  (`SsoRejected` → `LoginRejected` event → `auth_logs`).
- **Auth logging** — `AuthEventSubscriber` records login/logout/rejection
  events to `auth_logs`.
- **User management** — edit name/permissions, restore soft-deleted users
  (`users.view`).
- **Sessions** — list and revoke active sessions (`sessions.view`).
- **Log viewer** — browse and clear the application log file (`logs.view`).
- **MakeAdmin** — `php artisan auth:make-admin {email}` promotes a user.

## AI Retouch & Experiments

- **AI Retouch** (`ai-retouch.view`): dashboard + images pages for AI
  image-retouch usage data.
- **Experiments** (`experiment.view`): A/B-test experiment strategy charts,
  with experiment files stored under `storage/app/private/experiments`.