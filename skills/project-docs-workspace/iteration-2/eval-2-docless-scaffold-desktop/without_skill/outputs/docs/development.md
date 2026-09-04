# Development

## Prerequisites

| Tool | Version | Notes |
| --- | --- | --- |
| Go | 1.25 (from `go.mod`) | `go-version-file` used by CI |
| Node.js | 24 (`frontend/package.json` `volta`) | |
| Wails CLI | matching `github.com/wailsapp/wails/v2` in `go.mod` | Windows with WebView2 runtime |

On Windows, Wails needs the WebView2 runtime and, for CGO-free SQLite (`glebarez/sqlite`), no special compiler setup is required. Arm/other platforms are not targeted — this app is Windows-only.

## Development workflow

The frontend and backend are developed together through Wails:

```sh
wails dev
```

- Frontend Vite dev server (hot reload) per `wails.json` (`frontend:dev:watcher`).
- Go backend rebuilt on changes; the frontend is served from the dev server URL.
- Do not run `npm run dev` standalone — the Wails bindings (`wailsjs/`) need the backend bindings, and `Runtime`/`Window` APIs only exist inside the Wails environment.

Frontend-only development is possible in a browser for pure-UI work, but any call into `wailsjs/go/*` will fail without the backend.

## Frontend commands

```sh
cd frontend
npm install          # install dependencies
npm run dev          # Vite dev server (usually via wails dev)
npm run build        # type-check + production build
npm run lint         # ESLint
npm run format       # Prettier (writes src/)
npm test             # Vitest (unit tests)
npm run test:watch   # Vitest watch
npm run test:coverage
```

## Backend commands

```sh
go generate ./pkg/sysinfo/...   # regenerate pci.ids database (required before tests)
go test ./pkg/...               # run all backend tests
go vet ./...
```

Note: `go test ./pkg/...` without `go generate` first will fail if the checked-in `pkg/sysinfo/data/pci.ids.gz` is absent or stale — CI runs generate before testing.

## Building

```sh
wails build                                    # debug-ish default build
wails build -ldflags "-X main.buildVersion=v1.2.3"   # embed a version (semver)
```

The version string is parsed in `main.go` `init()` via `semver` and shown on the About page. Use the tag name (`v…`) as the ldflag value to match the release pipeline.

`wails build` produces `build/bin/install-it.exe` plus the frontend assets embedded into the binary.

### Runtime assets for a distributable build

A usable binary expects the following next to it at runtime (the release workflow assembles them):

- `internals/bin/WebView2/` — fixed-version WebView2 runtime. If absent, the app falls back to the system WebView2.
- `internals/data/pci.ids.gz` — PCI ID database for hardware name resolution.
- `internals/data/PawnIO_setup.exe`, `IntelMSR.bin`, `RyzenSMU.bin` — CPU temperature support (optional; temp polling reports unavailable without them).

`conf/` and `drivers/` are created automatically on first run.

## Release pipeline

GitHub Actions drive releases. Two workflows:

### `test.yml`

Runs on every PR to `main` and is reusable (`workflow_call`):

- **Go tests** (windows-latest): `go generate ./pkg/sysinfo/...`, then `go test ./pkg/...`.
- **Frontend tests** (ubuntu-latest): `npm ci`, `npm test`.

### `build_and_release.yml`

Triggered by pushing a tag `v*` (or manually). Steps:

1. Run tests (reuse `test.yml`).
2. Build matrix `goarch: 386, amd64` → `install-it.windows-x86/x64.exe` via `wails build -ldflags "-X main.buildVersion=${{ github.ref_name }}"`.
3. Assemble runtime assets per arch: WebView2 fixed-version runtime CAB (downloaded + expanded into `internals/bin/WebView2`), `pci.ids.gz`, PawnIO driver + modules.
4. Package two ZIPs per arch: `install-it.<os>-<arch>.zip` (exe + data) and `…-bundled.zip` (exe + full internals).
5. Emit `.sha256` checksums — the in-app updater verifies these before applying.
6. Build the separate updater executable (`install-it-updater` repo, PyInstaller) per arch.
7. Create the GitHub release with all artifacts; tags containing `-alpha`/`-beta`/`-rc` are marked prerelease.

Release asset naming is load-bearing: `CheckForUpdates` looks for exactly `install-it.{goos}-{goarch}.zip` and `-bundled.zip`, where `goarch` is mapped `amd64→x64`, `386→x86`.

## Testing notes

- Backend: stdlib `testing` throughout `pkg/`, including fuzz tests (`pkg/execute`, `pkg/storage`). The updater's integration test spins up an `httptest` server to fake the GitHub API and the downloadable assets.
- Frontend: Vitest + `@vue/test-utils` + jsdom. Notable suites: `decodeError`, `i18n-completeness` (all locales must contain the same keys), store tests, `useEditor`.
- Keep `testMatchRule` (frontend `src/utils/index.ts`) and `testRule` (backend `pkg/matching/matching.go`) in sync — both implement rule evaluation for their side of the UI.

## Code conventions

- Errors returned to the frontend must be `errcode.New`/`Newf` with a stable code that exists in both `frontend/src/i18n/*.json` files. Prefix warnings with `warn`, errors with `err`.
- JSON field names in Go structs become the TS bindings — keep them camelCase, matching frontend usage.
- DB schema changes are additive gormigrate migrations in `pkg/storage/db.go`, never AutoMigrate-only.