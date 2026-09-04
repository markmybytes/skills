# Build Directory

Static build assets for install-it (Wails). The release pipeline
(`.github/workflows/build_and_release.yml`) drives `wails build`; this
directory holds the assets it needs.

## Structure

- `bin/` — build output directory; CI also assembles release payloads here
  (`install-it.exe`, `internals/` with the WebView2 runtime, PCI ID database,
  PawnIO files, `.zip.sha256` checksums)
- `windows/` — Windows manifest, icon, and installer files used by `wails build`
- `darwin/` — macOS scaffold. **Unused**: install-it is a Windows-only app (see
  [AGENTS.md](../AGENTS.md)); retained as Wails default.
- `appicon.png` — application icon source

## Notes

- `windows/icon.ico` — application icon; if missing, it is regenerated from
  `appicon.png` on `wails build`.
- `windows/info.json` — application details used by the Windows installer and
  file properties (right click exe → properties → details).
- `windows/installer/` — files used to create the Windows installer.
- `windows/wails.exe.manifest` — main application manifest file.
- To restore Wails defaults, delete the files and rebuild with `wails build`.