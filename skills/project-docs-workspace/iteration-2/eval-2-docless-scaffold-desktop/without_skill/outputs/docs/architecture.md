# Architecture

install-it is a [Wails v2](https://wails.io) desktop application. The Go backend is the native side of the app; the Vue 3 frontend renders in a WebView2 view. All UI-to-backend calls cross the Wails bridge through generated TypeScript bindings in `frontend/wailsjs/`.

```
┌──────────────────────────────┐        Wails bridge        ┌──────────────────────────┐
│  frontend/ (Vue 3)           │  ───────────────────────►  │  Go backend               │
│  pages · stores · components │  wailsjs/ bindings         │  main.go · app.go · pkg/  │
└──────────────────────────────┘  ◄───────────────────────  └──────────────────────────┘
                                events: execute:exited
```

## Startup wiring

`main.go` initialises global singletons in `init()` (runtime dirs, version from `-ldflags -X main.buildVersion`, WebView2 path), then in `main()`:

1. Opens and migrates the SQLite database (`conf/data.db`).
2. Creates storages: `DriverGroupStorage`, `RuleSetStorage`, `AppSettingStorage`, and the `Matcher`.
3. Creates the shared `Porter` instance (with DB close/reopen hooks).
4. Applies any staged update (`update.ApplyStagedUpdates`) *before* Wails starts — WebView2 hasn't loaded internals yet, so no file locks exist.
5. Runs Wails with the bound services.

The `ErrorFormatter` converts backend errors to a structured `{code, params}` payload (see [pkg/errcode](#pkg-errcode)).

## Runtime directory layout

The app is portable: everything lives next to the executable (`dirRoot`).

```
dirRoot/
├── install-it.exe
├── conf/                  # created on startup
│   ├── data.db            # SQLite: driver groups, drivers, rule sets
│   └── setting.json       # app settings
├── drivers/               # created on startup
│   ├── network/
│   ├── display/
│   └── miscellaneous/
└── internals/             # optional bundled assets
    ├── bin/WebView2/      # fixed-version WebView2 runtime (used if present)
    └── data/              # pci.ids.gz, PawnIO_setup.exe, IntelMSR.bin, RyzenSMU.bin
```

## Backend packages

### `pkg/storage`

SQLite persistence through GORM.

- `Database` — wraps `gorm.DB` plus the file path so the connection can be closed and reopened after a Porter import replaces `data.db`. Migrations run via `gormigrate` (`2026052601_baseline`, `2026052901_m2m_and_uint_pks`).
- `DriverGroupStorage` — CRUD for driver groups, plus `Clone` and `MoveBehind` (position reordering). `Update` reconciles child drivers (creates new, saves changed, deletes removed) and rewrites incompatible-driver associations.
- `RuleSetStorage` — CRUD for rule sets, `Clone`; driver-group links stored as a many-to-many association.
- `AppSettingStorage` — settings persisted as JSON at `conf/setting.json`. Missing file is seeded with defaults; legacy files get backfilled defaults.

### `pkg/errcode`

Structured errors that cross the Wails bridge. Each error carries a stable `code` that doubles as the vue-i18n key, plus optional interpolation `params`. The Wails `ErrorFormatter` marshals `*errcode.Error` to `{"code": "...", "params": {...}}`; raw errors fall back to plain strings. Codes are prefixed `err` (errors) or `warn` (warnings) and the frontend colours toasts accordingly.

### `pkg/execute`

`CommandExecutor` runs external programs.

- `Run` — starts a command asynchronously, returns a generated ID. Emits `execute:exited` with the `CommandResult` when finished.
- `RunAndOutput` — synchronous variant, optionally hidden window (`CREATE_NO_WINDOW`).
- `Abort` — kills the process *and its children* via gopsutil.
- Output is decoded from the detected charset (chardet) before crossing the bridge.

### `pkg/matching`

`Matcher` evaluates all rule sets against live hardware (`WMIHardwareQuerier` via `pkg/sysinfo`). A rule set matches when its rules match any hardware input (or all, when `ShouldHitAll`); matched rule sets contribute their linked driver groups. The frontend's `utils.testMatchRule()` mirrors the Go `testRule` logic — changes must land in both places.

### `pkg/sysinfo`

Windows hardware enumeration:

- WMI queries for CPU, physical memory, baseboard, disk drives, and OS (`Caption`, `DisplayVersion`, activation state via `SoftwareLicensingProduct`).
- PnP device enumeration for GPU/NIC/Bluetooth names, preferring real driver names and falling back to the PCI ID database (`pci.ids.gz`, generated via `go generate`).
- Individual query errors are silently ignored — degraded results are preferred over failure.

### `pkg/cputemp`

Reads CPU package temperature through the PawnIO kernel driver. `Init` installs the driver if absent and loads the vendor module (IntelMSR / RyzenSMU). Reads happen via `DeviceIoControl` IOCTLs. Package state is atomic because `Init` runs in a goroutine while `GetCPUTemperatures` is polled. Windows-only.

### `pkg/porter`

ZIP export/import of app data. One job at a time.

- `Export` — writes `manifest.json`, `conf/`, `drivers/` into a ZIP.
- `ValidateZip` — reads the manifest (checks `format_version`) and previews contents: settings, database, driver count/size.
- `ImportFromFile` — validates, backs up existing files/dirs to `.porter-*`, extracts selected categories (settings / data), rolls back on extraction failure. Closes the DB before touching `data.db` and reopens + re-migrates afterwards.
- `ImportFromURL` — two-step: `DownloadAndValidate` fetches + validates into a temp file, then `ImportFromURL` consumes it.
- `RecoverOrphanedBackups` — restores files from leftover `.porter-*` dirs after interrupted imports; called at startup.

See [porter format](#porter-zip-format) below.

### `pkg/update`

Self-update against GitHub releases (`markmybytes/install-it`).

- `CheckForUpdates` — queries the releases API, resolves assets `install-it.<os>-<arch>.zip` and `...-bundled.zip`, compares against the current version (semver).
- `TriggerNativeUpdate` — downloads the chosen asset, verifies the `.sha256`, extracts to `.update_stage`, swaps `install-it.exe` (keeping `.old`), restarts. Path traversal in archives is rejected.
- `ApplyStagedUpdates` — runs at launch, before Wails initialises. Deploys staged `internals/` (WebView2 runtime, PCI IDs, PawnIO modules) with an idempotent two-phase commit and a crash-recovery state machine. `install-it.exe.old` presence discriminates a completed exe swap from an interrupted extract.

### `pkg/status`, `pkg/utils`

- `status` — task lifecycle enum: `pending, running, completed, failed, aborting, aborted, skiped, speeded, errored`. Bound to the frontend as `status.Status`.
- `utils` — generic slice helpers (`All`, `Some`, `Map`, `FlatMap`) and `VerifySHA256`.

## Frontend

Vue 3 single-page app in `frontend/`, hash routing via `vue-router/auto-routes` (file-based pages).

- `src/pages/` — `index` (install), `drivers/` (list, create, edit), `match-rules/` (list, create, edit), `settings`, `porter`, `system-utilities`, `app-info`.
- `src/stores/` — Pinia stores: `useDriverGroupStore`, `useMatchRuleStore`, `useAppSettingStore` (mirrors backend settings), `useUnsavedFormStore` (leave-without-saving guard).
- `src/composables/` — `useEditor` (form draft + dirty tracking), `useReorderable` (drag-sort via SortableJS), `useLoading`.
- `src/i18n/` — `en` and `zh_Hant_HK` (default). Backend error codes are looked up here.
- `src/utils/` — `getHardware`/`getOSInfo` (calls `SysInfo`), `testMatchRule` (must stay in sync with `pkg/matching`), `decodeError`.
- Wails bindings are generated into `src/wailsjs/`; `go/models.ts` holds the mirrored structs/enums.

## Data model

```
DriverGroup ──< Driver ──< (driver_incompatibles) ──> Driver
    │ type: network | display | miscellaneous
    │ mutuallyExclusive: if set, other drivers in the same group are treated as incompatible
    │ position: display order
    │
    └──< Driver
         name, path, flags[], minExeTime, allowRtCodes[], incompatibles[]

RuleSet ──< Rule                       RuleSet ──< DriverGroup (m2m)
  name                                     rule_set_driver_groups
  shouldHitAll (all rules vs any rule)
  Rule: { source, operator, isCaseSensitive, shouldHitAll, values[] }
```

- `Rule.source`: `cpu | motherboard | gpu | memory | nic | storage`
- `Rule.operator`: `contain | notContain | equal | notEqual | regex`
- A driver group's `mutuallyExclusive` flag is expanded into per-driver incompatibilities in the frontend before execution.

## Porter ZIP format

```
manifest.json          {"format_version": 1, "exported_at": "…"}
conf/setting.json      (optional) settings export
conf/data.db           (optional) database export
drivers/…              (optional) driver files
```

`ImportPreview` reports which categories exist plus driver count/size. Import rejects version-unsupported and empty archives. Data = database and/or drivers; the two can be imported independently of settings.

## Events

- `execute:exited` — emitted by `CommandExecutor.Run` when a command completes or is aborted. Payload: `{lapse, exitCode, stdout, stderr, error, aborted}`.