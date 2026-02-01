# TODO: Password Manager Implementation (Phase 1 — Desktop MVP)

## 🧱 Setup & Scaffolding
- [ ] Initialize Tauri + React project: `npm create tauri-app@latest` ✅ (done)
- [ ] Configure `src-tauri/Cargo.toml` with required crates (`ring`, `argon2`, `zeroize`, `serde`, `bincode`, `flate2`, `tauri-plugin-clipboard`, `tauri-plugin-dialog`)
- [ ] Set up Vite + TypeScript + Tailwind CSS in `src/frontend/`
- [ ] Define shared TS types (`Entry`, `VaultHeader`, `VaultConfig`)

## 🔐 Crypto & Vault Core (Rust)
- [ ] Implement `crypto.rs`: `derive_key(password, salt, params) -> Key`, `encrypt(data, key, iv) -> (ciphertext, tag)`, `decrypt(...)`
- [ ] Implement `zeroize` for `SecretString`, `SecretVec`
- [ ] Implement `vault.rs`: `open_vault(path, mode, password?, keyfile?) -> Result<Vault>`
- [ ] Implement `save_vault(vault, path, mode, password?, keyfile?)`
- [ ] Write unit tests for crypto (using `#[cfg(test)]`)

## 🖥️ Frontend Core
- [ ] Create Zustand store: `vaultStore.ts` (current vault, entries, loading state)
- [ ] Implement IPC service: `api.ts` (wrap `invoke('open_vault', ...)`, etc.)
- [ ] Build responsive layout with Tailwind: sidebar (vaults), main (entries), header (search, add)

## 🔑 Vault UI
- [ ] Vault creation modal (3 modes: pwd-only, pwd+key, key-only)
- [ ] Vault opening flow (file picker → password/key input → load)
- [ ] Vault settings (change password, export key file, etc.)

## 📋 Entry Management
- [ ] Entry list with search/filter (debounced)
- [ ] Add/edit entry form (title, user, pass, url, notes, tags)
- [ ] Password generator component (configurable)
- [ ] Copy-to-clipboard with timeout (use `tauri-plugin-clipboard`)

## 📤 Export/Import
- [ ] Export modal: choose format (JSON plaintext, JSON encrypted, CSV), confirm
- [ ] Import modal: select file, choose mode (plaintext/encrypted), validate
- [ ] Show warnings for CSV (plaintext) exports

## ⚙️ Preferences
- [ ] Settings panel: clipboard timeout, theme, password defaults
- [ ] Persist preferences to `~/.config/password-manager/config.json`

## 🧪 Testing & Polish
- [ ] Write React component tests (Jest + RTL) for EntryList, VaultManager
- [ ] Manual security review: no console.log of secrets, zeroize usage verified
- [ ] Build & test on Linux, Windows, macOS (via GitHub Actions or local)
- [ ] Write `README.md` with install/run instructions

## 📦 Release Prep
- [ ] Add icon assets (SVG + PNG 256x256)
- [ ] Configure Tauri bundler (`tauri.conf.json`) for all platforms
- [ ] Generate signed binaries (optional, for trust)

---