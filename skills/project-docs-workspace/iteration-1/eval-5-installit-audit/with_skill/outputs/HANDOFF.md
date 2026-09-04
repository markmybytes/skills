# pkg/update/ refactor record

This document records the outcome of the self-update package review and refactor: bugs found, decisions made, and the shipped architecture. The refactor is **complete** — the architecture and fixes below are implemented in the current code, not pending. Background: `pkg/update/` powers the in-app self-updater for the Wails v2 Windows desktop app — downloads release ZIP, verifies its SHA-256, swaps the executable, deploys `internals/` on next startup.

## Goal

Keep the updater package **simple, organized, and canonical**. No unnecessary complexity. Reuse existing patterns before adding new ones. Stdlib over dependencies. One way to do each thing. Prefer deletion over addition.

## Architecture (implemented)

| Phase | Where | Notes |
|---|---|---|
| `ApplyStagedUpdates(dirRoot)` | `main()` **before** `wails.Run(...)` (`main.go:114`) | No WebView2 yet → no internals locks. Idempotent, safe to re-run on every launch. |
| `TriggerNativeUpdate` | User click → `os.Exit(0)` (`pkg/update/update.go:142-217`) | Two-phase exe swap, runs in old exe context. |
| Update check | Frontend-triggered (`frontend/src/pages/app-info.vue` calls `CheckForUpdates`) | No file mutations. `OnStartup` does not check for updates. |

Pre-Wails apply avoids Windows file locks entirely:
- `.old` exe: WebView2 ghost processes from old exe can hold it briefly → retry loop (30s budget, 300 × 100ms).
- internals files: only locked when WebView2 starts loading them → free pre-Wails.
- new exe itself: locked (current process), but apply doesn't touch it.

## Deploy policy (implemented)

- **Swap phase** (`TriggerNativeUpdate`): `install-it.exe` only.
- **Apply phase** (`ApplyStagedUpdates`): `install-it.exe.old` cleanup + `internals/` two-phase replace.
- Unknown root entries in the staged zip are extracted but **silently ignored** by apply (only `internals/` is deployed; the whole stage dir is scrubbed afterwards). The earlier fail-loudly policy (`errUpdateUnexpectedRoot`) was deliberately removed in the simplification pass — extract accepts any root entry, and SHA-256 verification already covers tampered zips.

## Resolved bugs

All items below shipped in the current code.

### 1. Swallowed IO errors — extract + apply phases
**Extract phase**: every IO error is surfaced, abort on first failure (`extractZipToDir`, `pkg/update/update.go:316-364`). Half-extracted = corrupt deploy, no recovery.

**Apply phase**: recovery-state-machine failure paths are silent (best-effort) — the next launch retries. Exe already swapped, internals failure ≠ brick (app runs on old internals).

### 2. HTTP status check + timeout — `pkg/update/update.go:22, 33-38`
`http.Client{Timeout: 30 * time.Second}`; non-200 → `errUpdateInfoUnavailable` (`update.go:79-81, 98-100, 149-151`).

### 3. Checksum validation — `pkg/update/update.go:165-183`
CI ships `install-it.<build-name>.zip.sha256` alongside release assets (`build_and_release.yml`). Updater downloads both, verifies via `utils.VerifySHA256` before extract.

### 4. Stale `.update_stage` — `pkg/update/update.go:185-186`
`os.RemoveAll(stageDir)` before extract prevents leftover payload bleed.

### 5. Apply moved to pre-Wails — `main.go:114`, `pkg/update/update.go:226-314`
`CheckAndApplyUpdates` renamed to the free function `ApplyStagedUpdates(dirRoot)`, called from `main()` **before** `wails.Run(...)`. `init()` call dropped. `.old` retry loop bumped to **30s**.

### 6. Two-phase commit for internals apply — `pkg/update/update.go:270-290`
`live → .old` first, then `staged → live`. `.old` deleted only after both succeed. Best-effort rollback if phase 2 fails.

### 7. Idempotent state recovery — `pkg/update/update.go:238-268`
Apply is safe to re-run after crash. The state machine detects partial states (`staged` / `internals.old` / `internals`), and `install-it.exe.old` presence discriminates a pending deploy from an interrupted extract. Cases handled: finish mid-swap, discard interrupted-extract leftovers, restore from `.old`, drop orphaned backup.

### 8. RPC contract — `frontend/src/components/UpdateModal.vue:143`
`.then(Quit())` removed. `TriggerNativeUpdate(selectedUrl)` never resolves (calls `os.Exit(0)`); callers handle `.catch`/`.finally` only.

### 9. Deploy policy
Originally planned as a fail-loudly `errUpdateUnexpectedRoot` gate; **reverted by decision** in the simplification pass — unknown root entries pass through extract and are scrubbed with the stage dir (see Deploy policy above).

## Decisions

- **Semver parse failure** → silent no-update (user-confirmed).
- **Prerelease picker** → `/releases` list with `preferPreRelease`; drafts are excluded by GitHub.
- **Drafts vs prereleases** → separate GH concepts; drafts hidden entirely, prereleases flagged.
- **Internals overlay vs full replace** → full replace. Updater trusts the package; CI owns packaging.
- **`.old` retry budget** → 30s. Longer than the original 10s for slow WebView2 ghost cleanup.
- **Apply swallows internals IO errors** → user-confirmed. Stage cleanup proceeds, next cycle redownloads.

## Skipped (ponytail)

- Manifest file: add when payload grows past 2 zones.
- Testable spawn/exit seams for `TriggerNativeUpdate`: add when bricking becomes a real incident.

## Out of scope

- `pkg/porter/` import path — unrelated lifecycle (closes DB on import).