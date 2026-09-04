# STYLE.md Review — install-it (desktop-style-ws)

Audit mode, read-only. Findings against the project-docs skill's opinionated STYLE.md structure. No files modified; no files copied.

## STYLE.md

- `STYLE.md` Layout Breakpoints (L131-133) — claims "`md:` and `xl:` only; `sm:` and `lg:` are skipped", but code uses `hidden sm:inline` (DriverEditor.vue:255,281, a show/hide element the section itself claims in scope) and `lg:max-w-2xl` / `lg:text-sm` / `lg:size-8` / `lg:mt-4` (DriverFormComponent.vue:139, MatchRuleFormComponent.vue:73,115, ProgressStepper.vue:73,93,104); the rationale even admits `sm:` is "always-on" (`stale`) → correct the rule or list explicit exceptions
- `STYLE.md` i18n prefix table (L179) — `toast` row cites `toastSaved` / `toastNoUpdate`, but no `toast*` key exists in `en.json` or `zh_Hant_HK.json`; real toast calls use `msgSaved`, `msgNoUpdate`, `warnPathRequired`, `errCheckUpdateFailed` (`stale`) → drop the row or replace with keys that exist
- `STYLE.md` Async example (L110) — `t('toast.noHardwareInfo')` references a non-existent nested key and contradicts the section's own "no nesting, no dots" rule (`stale`) → use a real key, e.g. `msgNoUpdate`
- `STYLE.md` Frontend "Basics" (L52-57) — restates rules already enforced as errors in `frontend/eslint.config.js`: `<script setup lang="ts">`/no Options API (`vue/block-lang`, `vue/component-api-style`), type-based props/emits (`vue/define-props-declaration`, `vue/define-emits-declaration`), block order (`vue/block-order`), blank lines between template tags (`vue/padding-line-between-tags`) (`structure`) → point at lint config instead of restating mechanical rules
- `STYLE.md` Quick Review Checklist (L244-265) — restates nearly every rule above, including the lint-enforceable ones, doubling the maintenance surface; two of its items ("no intermediate variables", "Porter DB lifecycle") duplicate other sections (`structure`) → trim to non-mechanical, non-duplicated items
- `STYLE.md` Database bullet (L224) — "Porter closes and reopens the DB on import" duplicates `AGENTS.md` Key constraints (operational fact owned by AGENTS.md) (`duplicate`) → replace with a link to AGENTS.md
- `STYLE.md` Constraints (L46) — restates `npm run lint` / `npm run format` commands owned by `AGENTS.md` Commands; `go fmt` / `go vet` is repeated three times across L46, L204, L250 (`duplicate`) → keep in AGENTS.md, link from STYLE.md
- `STYLE.md` ownership declaration (L3) — present ("Canonical conventions for install-it") but does not state what links to another owner; operational facts that belong in AGENTS.md are instead duplicated here (see above) (`structure`, minor) → one line pointing to AGENTS.md for commands/architecture

## Adjacent (reference-only, not in scope)

- `frontend/README.md` — untouched Vite template boilerplate ("This template should help get you started developing with Vue 3 in Vite") (`stale`) → replace with project-specific sub-README or remove; not a STYLE.md issue

## Confirmed good

- Rule priority order present and matches default priority (Security → Correctness → Architecture → Simplicity → Local consistency), L11-17.
- No dead sections, no leftover empty rule bullets; fresh-scaffold-style placeholders absent (document is mature).
- Window-size facts verified: `main.go` sets `MinWidth: 640, MinHeight: 480`, `Width: 768, Height: 576` — matches L133-138.
- `sm:`/`lg:` rule aside, breakpoint rationale is evidence-based; the "tracked separately and not yet documented" carve-out for font/padding/container-width (L146) is an honest declared gap.
- i18n "flat camelCase, no nesting/dots" rule matches locale files; all other prefix examples (`msg`, `warn`, `err`, `step`, `statusShort`, `field`, `desc`, etc.) exist as keys.
- Wails-bindings constraint owned by STYLE.md (coding-practice/technical-contract per skill); AGENTS.md's copy is a permissible summary.

## Needs user-provided facts

- None. All findings are evidence-backed; no unresolved claims require user input.