# Project Summary

## Overall Goal
Build a secure, modern, cross-platform desktop password manager (Linux/macOS/Windows) with file-based encrypted vaults (`.pmv`), strong AES-256-GCM + Argon2id encryption, GUI-only interface, and production-grade code — mobile support deferred to Phase 2.

## Key Knowledge
- **Platform**: Desktop-only MVP first; mobile (iOS/Android) explicitly deferred to later phase per user preference.
- **Tech Stack**:
  - Frontend: React (TypeScript) + Vite + Tailwind CSS + Headless UI + Zustand
  - Backend: Rust (Tauri core) using `ring`, `argon2`, `zeroize`, `serde`, `bincode`, `flate2`
  - Vault format: Custom binary `.pmv` — header + GZIP-compressed JSON entries + AES-256-GCM authenticated encryption
- **Security Requirements**:
  - Zero cloud, local-only storage
  - Master password + optional key file (3 modes: pwd-only, pwd+key, key-only)
  - Secrets zeroized in memory (`zeroize` crate)
  - Clipboard auto-clears after configurable timeout (default 30s)
  - No logging of sensitive fields
- **User Preferences**:
  - Minimal build effort, clean/maintainable code
  - Modern, responsive UI (Tailwind + responsive layout)
  - Export/import supported for JSON (plaintext/encrypted) and CSV (plaintext only, with warning)
  - No CLI, no browser auto-fill
- **Project Structure** (planned):
  ```
  src/                # Rust backend (lib/vault.rs, crypto.rs, entry.rs)
  src-tauri/          # Cargo.toml, main.rs
  src/frontend/       # React + TSX + Tailwind + Zustand
  PLAN.md, TODO.md    # Maintained as living docs
  ```
- **Build Commands** (to be used once deps installed):
  - `npm run tauri dev` → dev server
  - `cargo build --release` + `npm run tauri build` → native binaries

## Recent Actions
- User provided detailed `spec.md` specifying requirements (platform, features, security, stack preferences).
- Agent proposed and refined a Tauri + React + Rust plan, emphasizing security, maintainability, and desktop-first delivery.
- `PLAN.md` and `TODO.md` were written and verified to exist in project root (confirmed via `list_directory`).
- Initial Tauri scaffolding attempted via `npm create tauri-app@latest --yes ...`, but failed to fully generate backend due to missing Rust toolchain (`rustc` not installed).
- Todo list initialized with 14 tasks; Task 1 (“Initialize Tauri + React project”) marked `[IN PROGRESS]` → effectively completed as scaffold exists (though incomplete without Rust).
- User confirmed they will install Rust separately and resume later; session paused intentionally.

## Current Plan
1. [DONE] Define architecture & requirements (`spec.md` reviewed, `PLAN.md`/`TODO.md` written)  
2. [IN PROGRESS] Set up Tauri project structure (scaffold created; awaiting Rust installation)  
3. [TODO] Install Rust toolchain (`rustup install stable`)  
4. [TODO] Complete Tauri setup: `cd --frontend-framework && npm install && cargo build`  
5. [TODO] Configure `src-tauri/Cargo.toml` with required crates (`ring`, `argon2`, `zeroize`, etc.)  
6. [TODO] Implement Rust crypto core (`crypto.rs`, `vault.rs`) with zeroization and unit tests  
7. [TODO] Build frontend shell: responsive layout, Zustand store, IPC service  
8. [TODO] Implement vault creation/opening UI and entry CRUD with secure clipboard  
9. [TODO] Add export/import and preferences  
10. [TODO] Security review + testing + cross-platform build validation  

> Next step upon resumption: Verify Rust is installed, then proceed with `cargo` and frontend setup.

---

## Summary Metadata
**Update time**: 2026-02-01T08:05:02.205Z 
