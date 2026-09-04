# install-it

A Windows desktop application for installing device drivers and running post-install setup tasks after a fresh OS installation. Built with [Wails v2](https://wails.io) (Go backend, Vue 3 frontend).

## What it does

- **Driver management** — define *driver groups* (network, display, miscellaneous), each holding one or more driver executables with their command-line flags, expected runtime and allowed exit codes.
- **Hardware matching** — define *rule sets* that match against live hardware (CPU, motherboard, GPU, memory, NIC, storage). One click pre-selects the driver groups that fit the current machine.
- **Bulk install** — runs the selected drivers sequentially or in parallel with live progress, abort support and incompatible-driver detection.
- **Pre/post setup tasks** — optional set-local-user password and RAW-disk partition creation before driver install, plus a configurable success action (nothing / shutdown / reboot / reboot-to-firmware).
- **Import/Export** — exports driver files, database and settings into a single ZIP (also importable from a URL). Imports back up existing files and roll back on failure.
- **Self-update** — checks GitHub releases, verifies SHA-256, stages and applies updates across restarts with crash recovery.
- **System tools** — shortcuts to Device Manager, Disk Management, Windows settings pages, and power actions.
- **Bilingual** — English and Traditional Chinese (zh_Hant_HK).

## Tech stack

| Layer | Technology |
| --- | --- |
| Desktop shell | [Wails v2](https://wails.io) (WebView2) |
| Backend | Go 1.25 |
| Storage | SQLite via GORM (`glebarez/sqlite`, `gormigrate`) |
| Frontend | Vue 3, TypeScript, Vite, [Nuxt UI](https://ui.nuxt.com), Pinia, vue-router, vue-i18n, Tailwind CSS |
| Tests | Go stdlib + Vitest |
| Release | GitHub Actions (Windows x86/x64) |

## Repository layout

```
.
├── app.go               # Wails App binding: dialogs, paths, app info
├── main.go              # Wails app entry point, startup wiring, bindings
├── wails.json           # Wails project config
├── pkg/
│   ├── cputemp/         # CPU temperature via PawnIO kernel driver
│   ├── errcode/         # Structured errors crossing the Wails bridge
│   ├── execute/         # Async command runner with abort + charset decoding
│   ├── matching/        # Rule-set evaluation against live hardware
│   ├── porter/          # ZIP export/import of config + drivers
│   ├── status/          # Task status enum
│   ├── storage/         # SQLite models, app settings, migrations
│   ├── sysinfo/         # WMI/PnP hardware enumeration, PCI ID resolution
│   ├── update/          # GitHub releases check + staged self-update
│   └── utils/           # Shared helpers
└── frontend/            # Vue 3 application
    └── src/
        ├── pages/       # install, drivers, match-rules, settings, porter, utilities, about
        ├── components/  # modals, forms, status displays
        ├── stores/      # Pinia stores (driver groups, settings, match rules, forms)
        ├── i18n/        # en / zh_Hant_HK
        └── wailsjs/     # generated Wails bindings
```

## Documentation

- [Architecture](docs/architecture.md) — backend packages, frontend structure, Wails bridge, data model
- [Development](docs/development.md) — prerequisites, dev workflow, building, testing, release pipeline
- [Usage](docs/usage.md) — user-facing workflow, settings reference, import/export format, error codes

## Quick start (development)

Requires Go 1.25+, Node.js 24, and the Wails CLI.

```sh
# install Wails CLI (pinned to the go.mod version)
go install github.com/wailsapp/wails/v2/cmd/wails@v2.12.0

# run in dev mode (frontend hot reload + Go rebuild)
wails dev

# build for the current platform
wails build

# tests
cd frontend && npm test          # frontend (Vitest)
go test ./pkg/...                # backend
```

See [Development](docs/development.md) for full details.

## License

GNU General Public License v2.0. See [LICENSE](LICENSE).