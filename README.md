# RDP Manager

Windows Remote Desktop manager with an Active Directory-style tree UI,
DPAPI-encrypted credentials, and a custom frameless window.

**License:** GNU General Public License v2.0 (GPL-2.0)
**Version:** 1.2.0

---

## Features

- Console tree — organise servers into sites with expand/collapse and context menus
- Secure credentials — DPAPI via `electron.safeStorage` (same as Chrome/VS Code/1Password).
  Encrypted per-machine and per-user. Never exported.
- RDP launch — injects credentials via `cmdkey`, launches `mstsc.exe`, auto-cleans up
- Server panel — sortable table, search filter, detail pane
- Export as XML — human-readable config backup (no credentials)
- Export as Server List — plain `.txt` file usable on any machine without the app
- Import — accepts XML (v1.1+) or legacy JSON
- Light and dark themes, persisted across restarts
- Portable ZIP or NSIS installer

---

## Build

**Only prerequisite:** [Node.js 18+](https://nodejs.org) — nothing else needed.

1. Extract the source ZIP anywhere (local drive recommended)
2. Double-click **`BUILD.bat`**

Output appears in `release\`:

| File | Description |
|---|---|
| `RDP Manager Setup 1.2.0.exe` | Full NSIS installer |
| `RDP Manager-1.2.0-win.zip` | Portable, no install required |
| `build.log` | Full build log — upload here if something fails |

---

## Export formats

### XML export
Human-readable backup of all sites and servers. Can be imported on another
machine running RDP Manager. Credentials are never included.

### Server List (plain text)
A `.txt` file listing every site and server with hostname, port, username,
description, and last-connected date. Useful as a reference on machines
where the app is not installed.

---

## Security model

| What | How |
|---|---|
| Credential encryption | `electron.safeStorage` → Windows DPAPI |
| Storage | `%APPDATA%\rdp-manager\credentials.json` (hex ciphertext) |
| Decryptable by | Same Windows user on same machine only |
| Exported? | Never — both export formats strip all credentials |
| In-memory lifetime | Decrypted only during `mstsc.exe` launch, then discarded |
| Injection method | `cmdkey /add` → mstsc → `cmdkey /delete` (8 s cleanup) |

---

## Data locations

| File | Path |
|---|---|
| Config | `%APPDATA%\rdp-manager\rdp-manager-config.json` |
| Credentials | `%APPDATA%\rdp-manager\credentials.json` |

---

## Project structure

```
src/
  main/
    index.ts                    Electron main process, BrowserWindow
    preload.ts                  Context bridge (window.electronAPI)
    handlers/
      configHandlers.ts         Load/save/export(XML+TXT)/import config
      credentialHandlers.ts     DPAPI encrypt/decrypt via safeStorage
      rdpHandlers.ts            RDP file generation, mstsc.exe launch
  renderer/
    App.tsx                     Root component, modal orchestration
    store/AppContext.tsx         Global state (useReducer + auto-save effect)
    components/                 TitleBar, Toolbar, SiteTree, ServerPanel,
                                StatusBar, ToastContainer, modals/
    styles/global.css           CSS variables, light + dark themes
  shared/
    types.ts                    Shared TypeScript interfaces + IPC constants
installer/
  setup.nsi                     Custom NSIS script (advanced use)
assets/
  icon.ico                      Multi-resolution icon (16/32/48/256 px)
BUILD.bat                       One-click build — double-click to run
```

---

## Development

```cmd
npm install
npm run dev
```

Starts the webpack dev server (port 3000) and Electron with hot reload.

---

## License

This program is free software: you can redistribute it and/or modify it
under the terms of the GNU General Public License as published by the
Free Software Foundation, either version 2 of the License, or (at your
option) any later version.

See [LICENSE.txt](LICENSE.txt) or <https://www.gnu.org/licenses/gpl-2.0.txt>.
