1. Platform & UI:
- Desktop app : Yes
- CLI-only : No, no CLI, only GUI
- Web app : File system will be used (same as keepass, we can create file of some passwords, another file of some another password and so on, so that each user will be able to keep his/her password along using the file)
- Mobile : Yes

2. Core Features:
- Password storage (with title, username, password, URL, notes)
- Strong encryption (AES-256 recommended choose most secure and supports on all platforms)
- Master password + optional 2FA (TOTP, recovery key): The passwords file can be encrypted using either only master password, or master password + key file, or a key file (separate from main passwords file)
- Search & filtering: Yes
- Copy-to-clipboard (secure, no logs): Yes, with clipboard clearance timeout (preferences can be set)
- Export/import (JSON, CSV — encrypted or plaintext?): Yes for all, user can choose the export either encrypted or plaintext
- Auto-fill (browser extension? not required for personal use): Not required (clipboard feature is enough)

3. Security Requirements:
- Zero cloud — Data stored in file (same as keepass, each file is encrypted)
- Memory safety: avoid logging secrets, zeroize sensitive buffers? : Don't know what to choose
- Key derivation: PBKDF2, Argon2, or scrypt?: Don't know what to choose
- Database format: SQLite? JSON file? Binary blob?: File system, same as keepass

4. Tech Stack Preference (if any):
- Frontend: React, Vue, Svelte, Tauri + HTML/CSS/JS, or native GUI (Qt, GTK)?: Please choose recommended platform, it should support Desktop (linux, windows, mac) + Mobile (iphone + android)
- Backend/crypto: Python (cryptography, pycryptodome), Rust (ring, sodiumoxide), Node.js (crypto), Go?: Don't know what to choose, it should be cross platform
- Build tool: npm, cargo, pip, etc.?: Don't know, choose based on the tech stack

Important notes:
- Build efforts should be minimal
- Code should be clean and neat, should be managable and maintainable
