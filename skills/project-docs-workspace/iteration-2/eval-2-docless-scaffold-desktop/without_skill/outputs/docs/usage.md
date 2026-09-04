# Usage

install-it installs device drivers and runs setup tasks after a fresh Windows installation. The app is portable: extract the ZIP anywhere (or use the bundled build), then run `install-it.exe`. All data lives next to the executable in `conf/` and `drivers/`.

## Workflow overview

1. **Create driver groups** (Drivers page) — group installer executables by category: *network*, *display*, *miscellaneous*. Each driver defines:
   - executable path (absolute, or relative to the app root)
   - command-line flags
   - minimum execution time (a driver that finishes faster is treated as suspicious)
   - allowed exit codes
   - incompatible drivers (other drivers that must not be installed alongside it)
   - **mutually exclusive** groups — the frontend automatically treats every other driver in the same group as incompatible with the selected one.
2. **(Optional) Define match rules** (Match Rules page) — bind driver groups to hardware signatures. Each rule targets a hardware source (`cpu`, `motherboard`, `gpu`, `memory`, `nic`, `storage`) with an operator (`contain`, `not contain`, `equal`, `not equal`, `regex`) and a list of values, with per-rule case-sensitivity and all/any semantics. A rule set links to one or more driver groups.
3. **Install** (Home page) — the hardware is detected and shown. Use **Match** to pre-select groups matching the detected hardware, or pick groups manually. Optionally enable the pre-install tasks and success action, then **Execute**.
4. Watch progress, abort if needed, and let the success action (shutdown / reboot / firmware) finish the job.

## Home page (Install)

- Shows detected hardware sections: CPU (with live temperature badge when enabled), GPU, memory, motherboard, NIC, storage, OS (caption, version, activation state).
- **Network** and **Display** are single-choice selects; **Miscellaneous** allows multiple selections.
- **Match** calls the backend matcher and pre-selects the driver groups whose rule sets match the current hardware.
- Pre-install tasks (checkboxes):
  - **Create partitions** — initialises RAW disks: `Initialize-Disk` → `New-Partition` → `Format-Volume`.
  - **Set password** — sets the current user's password (`Set-LocalUser`); empty password field clears the password.
  - **Parallel install** — run driver tasks concurrently instead of sequentially.
- **Success action** — after all tasks complete: nothing / shutdown / reboot / reboot to firmware, with a delay in seconds.

If the driver executable is missing for a group, the group is still listed (unless **Hide not found** is enabled in settings); execution will report the failure per task.

## Drivers page

- Filter by category tabs (network / display / miscellaneous).
- Drag to reorder groups (sort toggle); groups persist their display order.
- Inspect a group to view drivers, flags, allowed exit codes and incompatibilities.
- Create / edit / clone / delete groups and their drivers.
- Driver groups are stored in `conf/data.db` (SQLite).

## Match Rules page

- A rule set contains one or more rules and links to driver groups.
- Rule semantics:
  - **Any / All** at rule level: whether *any* value must match (default) or *all* values must match.
  - **All rules must match / Any rule matches** at rule-set level.
  - Regex rules are compiled case-sensitively or with `(?i)` prefix depending on the case-sensitivity toggle.
- Hardware strings come from the same enumeration shown on the Home page, so values like `Intel(R) Core™ i7-13700K` can be matched with `contain` + `i7-13700K`.

## Import / Export page

Produces and consumes a single portable ZIP containing any combination of:

- **Settings** — `conf/setting.json`
- **Data** — the database `conf/data.db` (driver groups, match rules) and/or the `drivers/` tree (the actual installer files)

Export: choose a destination folder, export creates `manifest.json` + selected categories.

Import:

1. **From file** — pick a local ZIP and **Validate**.
2. **From URL** — the URL defaults to `driver_download_url` from settings; **Update** downloads and validates.
3. The preview shows what the archive contains (settings, database, driver count/size), with warnings when data is one-sided (drivers without database, database without drivers).
4. Select the categories to import (settings / data), then import.

Import behaviour:

- Existing files are backed up first (`.porter-*` folders) and restored if extraction fails.
- The database is closed before replacement and reopened + re-migrated afterwards.
- Archives with an unsupported `format_version` or with no recognised content are rejected.
- Interrupted imports are recovered automatically on the next launch.

## System Utilities page

- MMC consoles: Computer Management, Device Manager, Disk Management.
- Windows Settings pages: Settings, Activation, Windows Update, Apps, Wi-Fi, Bluetooth.
- Power actions: Shutdown / Restart / Restart to UEFI firmware — each requires confirmation.

## Settings

Persisted in `conf/setting.json`.

| Setting | Default | Description |
| --- | --- | --- |
| `auto_check_update` | `true` | Check for updates on launch |
| `allow_pre_release` | `false` | Include pre-release builds when checking |
| `driver_download_url` | — | Default import URL on the Import/Export page |
| `create_partition` | `false` | Default state of the "create partitions" pre-install task |
| `set_password` | `false` | Default state of the "set password" pre-install task |
| `password` | — | Password to apply (empty = clear password) |
| `parallel_install` | `true` | Default state of parallel install |
| `success_action` | `nothing` | Default success action |
| `success_action_delay` | `5` | Delay in seconds before the success action runs |
| `language` | `en` | `en` or `zh_Hant_HK` |
| `enable_cpu_temp` | `false` | Show CPU temperature badge on the Home page |
| `cpu_temp_refresh_interval` | `5` | Temperature poll interval in seconds |
| `hide_not_found` | `false` | Hide driver groups whose executables are missing |

Settings take effect on the Home page immediately (they live in the Pinia store and are copied back to `setting.json` when saved).

## Updates (About page)

- **Check for updates** queries the GitHub releases for `install-it` and compares semver against the running build.
- When `internals/` exists (bundled build), a "prefer bundled" update (full internals ZIP) is offered.
- Downloading verifies the release asset's `.sha256` before staging.
- The update is applied across a restart: the new executable is swapped immediately, and staged `internals/` (WebView2 runtime, PCI IDs, PawnIO modules) are deployed on the next launch. A crash mid-deploy is recovered automatically.

## Error messages

Backend errors cross the bridge as `{code, params}`. The code is a translation key; `err…` codes render as error toasts, `warn…` as warning toasts. If a code is missing from the locale file, the fallback locale (`en`) is used.

## Notes / limitations

- Windows-only (uses WMI, PnP, registry, WebView2, and Windows `shutdown`/PowerShell commands).
- Driver executables are run as-is; the app does not verify signatures or match installer types — review a driver group before executing on a production machine.
- CPU temperature requires the PawnIO kernel driver; it is only available on supported CPUs (Intel/AMD) and may require a reboot after first install.