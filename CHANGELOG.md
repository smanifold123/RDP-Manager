# Changelog

## [1.2.0] - 2026-04-25

### Changed
- Toolbar redesigned: Import, Export, and Server List moved into a **File** dropdown
  menu; Settings and theme toggle moved into a **View** dropdown menu — removes
  clutter from the toolbar and groups related actions logically
- Theme can now be toggled directly from the View menu without opening Settings
- "Export XML" renamed to "Export" throughout

---

## [1.1.0] - 2026-04-24

### Fixed
- Theme preference now persists correctly across restarts via a dedicated
  auto-save effect that fires on every settings change
- XML import: server and site names were silently lost due to a tag mismatch
  (`<n>` written, `<name>` expected) — both sides now use `<name>`
- BUILD.bat: output detection uses `dir /s /b` (handles filenames with spaces)
- NSIS VIProductVersion and copyright year updated

### Added
- Export as XML — human-readable config backup (no credentials)
- Export as Server List — plain `.txt` usable on machines without the app
- Import accepts XML (new) and legacy JSON

### Changed
- License changed from MIT to GPL-2.0
- Version bumped to 1.1.0

---

## [1.0.0] - 2026-04-24

### Added
- Initial release
- Active Directory-style console tree (sites → servers)
- DPAPI credential storage via `electron.safeStorage`
- Frameless custom titlebar (minimize / maximize / close)
- Toolbar: Connect, New Site, Add Server, Properties, Delete, Refresh
- Server panel with sortable table, search filter, and detail pane
- Add/Edit Server modal with General, Credentials, and Advanced tabs
- Settings modal: theme, default port, RDP options, security info
- Light and dark theme support
- Config at `%APPDATA%\rdp-manager\rdp-manager-config.json`
- Credentials (encrypted) at `%APPDATA%\rdp-manager\credentials.json`
- RDP launch via `mstsc.exe` with `cmdkey` injection and auto-cleanup
- NSIS installer + portable ZIP build
- Option A icon (layered screens with forward-arrow badge)
- BUILD.bat one-click build script with UNC path support and build logging
