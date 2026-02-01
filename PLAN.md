# Password Manager — Implementation Plan (v1.0)

## Goal
Build a secure, modern, cross-platform (desktop: Linux/macOS/Windows) password manager with file-based vaults, strong encryption, and a responsive UI. Mobile support deferred to Phase 2.

## Non-Goals (Phase 1)
- CLI interface
- Browser auto-fill
- Cloud sync
- Biometric unlock (mobile only, Phase 2)
- Advanced sharing/collaboration

## Tech Stack

### Frontend
- **Framework**: React 18 + TypeScript
- **Styling**: Tailwind CSS (v3.4+) + `@headlessui/react` for accessible components
- **State**: Zustand (minimal, typed, no context bloat)
- **Build**: Vite (fast dev server, optimized production build)

### Backend (Tauri Core)
- **Language**: Rust (1.75+)
- **Crates**:
  - `ring`: AES-256-GCM, PBKDF2, HKDF
  - `argon2`: Argon2id (v19) for KDF
  - `zeroize`: Secure memory wiping
  - `serde` + `bincode`: Serialization (header + encrypted payload)
  - `flate2`: GZIP compression of entry data
  - `tauri-plugin-clipboard`: Secure clipboard (auto-clear)
  - `tauri-plugin-dialog`: File open/save dialogs
- **Vault Format**:
  ```
  [8B magic] "PMV1"
  [1B version] 1
  [1B cipher] 1 = AES-256-GCM
  [1B kdf] 1 = Argon2id
  [4B salt len] 16
  [16B salt]
  [4B ops] (Argon2 params)
  [4B mem] 
  [4B parallel]
  [16B iv]
  [N B encrypted payload] → GZIP(JSON(entries)) encrypted with AES-256-GCM
  ```

### Security Guaranteces
- Master password never stored in memory longer than needed
- All secret buffers (`Vec<u8>`, `String`) implement `Zeroize`
- Clipboard auto-clears after 30s (configurable)
- No logs of passwords, URLs, or usernames
- Vault integrity verified via GCM tag

## MVP Features

1. **Vault Management**
   - Create new vault (3 modes: pwd-only, pwd+keyfile, keyfile-only)
   - Open existing vault
   - Change master password / key file

2. **Entry CRUD**
   - Add/edit/delete entries (title, username, password, URL, notes, tags)
   - Password generator (length 8–32, include digits/symbols/uppercase)

3. **Search & Filter**
   - Real-time search across title, username, URL, notes
   - Tag filtering

4. **Clipboard**
   - Copy password → auto-clear after timeout (default 30s)
   - “Copy username” also supported

5. **Export/Import**
   - Export: JSON (plaintext or encrypted with vault key), CSV (plaintext only — warning shown)
   - Import: JSON (encrypted/plaintext), CSV (plaintext)

6. **Preferences**
   - Clipboard timeout (5s–300s)
   - Default password length & charset
   - Theme (system/light/dark)
   - Auto-lock after idle (optional future)

## Project Structure

```
password-manager/
├── src/                     # Rust backend (Tauri)
│   ├── main.rs              # Tauri command handlers
│   └── lib/
│       ├── vault.rs         # Vault open/save/encrypt/decrypt
│       ├── crypto.rs        # KDF, AES, zeroize helpers
│       └── entry.rs         # Entry schema & serialization
├── src-tauri/
│   └── Cargo.toml           # deps: ring, argon2, zeroize, tauri, etc.
├── src/frontend/            # React + TSX
│   ├── public/
│   ├── src/
│   │   ├── main.tsx
│   │   ├── App.tsx
│   │   ├── stores/          # Zustand stores
│   │   │   └── vaultStore.ts
│   │   ├── components/
│   │   │   ├── ui/          # Reusable Tailwind components
│   │   │   ├── VaultManager.tsx
│   │   │   ├── EntryList.tsx
│   │   │   └── ...
│   │   ├── services/
│   │   │   └── api.ts       # Tauri IPC wrappers
│   │   └── types.ts         # Shared TS interfaces
│   └── vite.config.ts
├── LICENSE
├── README.md
├── PLAN.md                  # ← this file
└── TODO.md                  # Task list (below)
```

## Phases

- **Phase 1 (MVP)**: Desktop app, vault CRUD, search, clipboard, export/import
- **Phase 2 (Optional)**: Mobile (Tauri Mobile), biometrics, auto-lock, browser extension

---