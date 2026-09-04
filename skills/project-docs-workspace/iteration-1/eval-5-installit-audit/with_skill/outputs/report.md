# Documentation audit — install-it

Audited against the `project-docs` skill (audit mode). Repo fixture used:
`installit-style-ws` (the path named `installit-audit-ws` does not exist in
`project-docs-evals/fixtures`; `installit-style-ws` is the only fixture with the
described doc layout: README, AGENTS, STYLE, HANDOFF, `readme/` translation,
`frontend/README.md`, `build/README.md`).

Fact-checked against repository evidence: `go.mod`, `main.go`, `app.go`,
`app_test.go`, `frontend/package.json`, `.github/workflows/*.yml`,
`pkg/update/update.go` + tests, `pkg/storage/`, `LICENSE`. `.slim/deepwork/`
is gitignored scratch (reference-only, not edited) and was used to confirm
HANDOFF staleness.

Findings, grouped by document, ordered by severity.

## HANDOFF.md

- `HANDOFF.md` — entire document describes a refactor still pending ("working checklist", "Bugs to fix") → the refactor is **complete** in code (`ApplyStagedUpdates(dirRoot)` called from `main.go:114` pre-Wails, two-phase internals commit, idempotent recovery state machine, 30s retry loop, SHA-256 verification, HTTP timeout + status check, `.then(Quit())` removed at `UpdateModal.vue:143`) → rewrite as a completed-refactor record (`stale`)
- `HANDOFF.md` — "Deploy policy (final)" and bug #9 document an `errUpdateUnexpectedRoot` fail-loudly gate for non-`internals` root entries → deliberately **removed** in the simplification pass: `extractZipToDir` passes unknown roots through, apply only deploys `internals/`, stage is scrubbed afterwards, i18n keys deleted → delete the policy bullet and mark #9 reverted (`stale`)
- `HANDOFF.md` — line references outdated: `update.go:69-90, 131-136` (#2), `update.go:217-225, 243, 247, 253-256` (#1), `update.go:149-153` (#4), `update.go:181-225` (#6) → current file is 365 lines; `TriggerNativeUpdate` at 142-217, `ApplyStagedUpdates` at 226-314, checksum at 165-183 → update references (`stale`)
- `HANDOFF.md` — Architecture table row "OnStartup | Post-Wails | Check-for-update only" → `main.go` `OnStartup` does chdir fix, `RecoverOrphanedBackups()`, and cputemp init; no update check. Update check is frontend-triggered (`app-info.vue` calls `CheckForUpdates`) → correct the row (`stale`)
- `HANDOFF.md` — item 5 says "Rename `CheckAndApplyUpdates` → `applyStagedUpdates`" → done as free function `ApplyStagedUpdates(dirRoot string)`; stale shim binding `CheckAndApplyUpdates` remains only in generated `frontend/wailsjs/go/update/Updater.*` (regenerated on next Wails build, never called from frontend) → mark resolved (`stale`)

## README.md

- `README.md` — "MIT License" badge (alt text) → repo `LICENSE` is GNU GPL-2.0 → change to "GPL-2.0" (`stale`)
- `README.md` — Prerequisite "Go ≥ 1.23" → `go.mod` requires `go 1.25.0` → "Go ≥ 1.25" (`stale`)
- `README.md` — Prerequisite "Node 22" → `frontend/package.json` pins Node 24.15.0 (Volta) and uses `@tsconfig/node24` → "Node ≥ 22, frontend pins 24.15.0" (`stale`)
- `README.md` — "Place all your installer under the `driver/<category>` folder" → code creates `drivers/` (`main.go` `init()`: `drivers/network`, `drivers/display`, `drivers/miscellaneous`) → `drivers/<category>` (`stale`)
- `README.md` — link "See [Exection Option](#exection-option)" → heading is "### Execution Option"; anchor `#exection-option` is dead → fix text and anchor to `#execution-option` (`structure`)
- `README.md` — English language link `https://github.com/markmybytes/install-it//README.md` has a double slash → `.../install-it/blob/master/README.md` (`structure`)
- `README.md` — typos: "categroies", "equivlent" (×2), "slient" → "categories", "equivalent" (×2), "silent" (`structure`)
- `README.md` — no documentation map; AGENTS.md, STYLE.md, HANDOFF.md, frontend/README.md exist but are unlinked → add a concise doc list under Getting Started (`gap`)
- `README.md` — CONTRIBUTING.md is a baseline doc and does not exist in the repo → recommend scaffolding (`gap`)

## readme/README.zh_Hant.md

- `readme/README.zh_Hant.md` — stray characters break rendering: line 8 `[forks-url]s`, line 14 bare `Ks` → remove both (`structure`)
- `readme/README.zh_Hant.md` — wrong character 軀 (body) used in 軀動程式 across tagline, body, NOTE, and table header → should be 驅動程式 everywhere (`structure`)
- `readme/README.zh_Hant.md` — "毋須進行進何操作" → "毋須進行任何操作"; "未有在上述表格中例出" → "列出" (`structure`)
- `readme/README.zh_Hant.md` — NOTE links `driver-claw` while English README links `it-claws` → align with English (`stale`)
- `readme/README.zh_Hant.md` — NOTE wording drifted: "並沒有內置任何驅動程式" (bundles no drivers) vs English "does not include any driver installers in releases" → align meaning with English (`stale`)
- `readme/README.zh_Hant.md` — `driver/<category>` → `drivers/<category>` (same as English) (`stale`)
- `readme/README.zh_Hant.md` — English link double slash `install-it//README.md` → fix (`structure`)
- `readme/README.zh_Hant.md` — unclosed `<p align="right">` after the About screenshot → close the tag (`structure`)
- `readme/README.zh_Hant.md` — Go/Node prerequisites carry the same stale versions as English → Go ≥ 1.25, Node ≥ 22 (pin 24.15.0) (`stale`)
- `readme/README.zh_Hant.md` — no documentation map → mirror the English doc list (`gap`)

## AGENTS.md

- `AGENTS.md` — Key constraints: "only `extractBinaryFromZip` is unit-tested" → the ZIP helper is `extractZipToDir` (`pkg/update/update.go`, tests `TestExtractZipToDir*`) → fix the name (`stale`)
- `AGENTS.md` — Commands comment "root package tests need Wails runtime" → false; `app_test.go` runs plain package-main tests and its own comment states `init()` side effects are "harmless for testing" → correct the rationale (`stale`)
- `AGENTS.md` — no documentation map table near the top (required by the skill) → add one (`gap`)
- `AGENTS.md` — CI: test workflow also runs frontend `npm test` on Ubuntu → only the Go job is described (`gap`)
- `AGENTS.md` — repository map lacks test locations (`frontend/src/__tests__/`, `app_test.go`) (`gap`)
- `AGENTS.md` — repo-workflow boundaries (workflows/vendor/wailsjs/secrets, lint-first) live only in STYLE.md "Constraints"; AGENTS.md is the boundary owner → add a Boundaries section (`duplicate`)

## STYLE.md

Review-only per the skill (STYLE.md content is never authored/rewritten by this
skill; no corrected file produced — fixes require maintainer action).

- `STYLE.md` — duplicates operational facts owned by AGENTS.md: "New Go structs... `Bind`/`EnumBind`" (dup of AGENTS Key constraints), "glebarez/sqlite (CGO-free)" and "gormigrate migrations in `pkg/storage/db.go`" (dup of AGENTS Architecture), "Porter closes and reopens the DB on import" (dup of AGENTS Key constraints) → link to AGENTS.md instead of copying (`duplicate`)
- `STYLE.md` — "Constraints" section holds repository-workflow boundaries (`.github/workflows`, `vendor`, `node_modules`, `wailsjs`, secrets) that belong in AGENTS.md → move to AGENTS.md Boundaries (`duplicate`)
- `STYLE.md` — mechanical, lint-enforceable rules restated instead of pointing at tool config: `vue/padding-line-between-tags`, "Run `npm run lint`/`npm run format` first", "`go fmt`/`go vet`" → point at `eslint.config.js` / `.prettierrc.json` (`structure`)
- `STYLE.md` — "Font-size scaling, padding density, and container widths are tracked separately and not yet documented" — unresolved open note in a mature STYLE.md → resolve or remove (`structure`)
- `STYLE.md` — rule priority order present (Security → Correctness → Architecture → Simplicity → Local consistency) ✓; ownership declaration present ✓; no dead sections for non-existent layers ✓ (`clean`)

## frontend/README.md

- `frontend/README.md` — stock Vite/Vue template boilerplate ("This template should help you get started developing with Vue 3 in Vite") → inaccurate for this Wails-managed frontend: `npm run dev` alone does not work (no Wails bridge/bindings); missing `npm test`, `npm run type-check`, `npm run format`, i18n; no links to AGENTS/STYLE → rewrite as a real frontend README (`stale`)

## build/README.md

- `build/README.md` — Wails default boilerplate describing macOS `darwin`/`Info.plist` → the app is Windows-only (per AGENTS.md); omits the actual CI-driven release assembly (WebView2 runtime, PCI ID database, PawnIO, `.sha256`) → rewrite minimal and factual (`stale`)

## Items needing user-provided facts

1. `it-claws` (English README) vs `driver-claw` (zh-Hant README) — only one can be correct; I aligned zh-Hant to the English `it-claws`. Confirm which companion tool actually exists.
2. README profile assumed **user-facing** (end-user install tool). Confirm if a developer-facing profile is preferred.
3. Whether STYLE.md "Constraints" items should be removed from STYLE.md now that AGENTS.md owns them (maintainer decision — the skill does not rewrite STYLE.md content).
4. CONTRIBUTING.md does not exist — scaffold is recommended but not created in audit mode; confirm before creating.

## Files produced in outputs/

| File | Action |
|---|---|
| `README.md` | corrected (stale facts, typos, links, doc map) |
| `readme/README.zh_Hant.md` | corrected (typos, wrong characters, stale facts, translation drift) |
| `AGENTS.md` | corrected (doc map, stale names/claims, boundaries, CI) |
| `HANDOFF.md` | rewritten as completed-refactor record |
| `frontend/README.md` | rewritten (removed boilerplate) |
| `build/README.md` | rewritten (removed boilerplate) |
| `STYLE.md` | **not produced** — review-only per skill; findings above require maintainer action |