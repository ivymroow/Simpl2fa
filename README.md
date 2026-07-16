# simpl2fa

a simple, local-first 2fa (two-factor authentication) desktop application built with electron. all data is stored locally on your machine with aes-256-gcm encryption. no cloud sync, no external dependencies, no telemetry.

---

security and trust

for transparency about this application's safety, see the [virustotal scan](virustotalscan.md).

---

technology stack

simpl2fa is built with electron (chromium + node.js) to provide cross-platform desktop support. the user interface uses vanilla html, css, and javascript with the web crypto api for totp code generation and aes-256-gcm for vault encryption.

architecture

the application uses a two-process model for enhanced security:

- main process: runs on node.js and handles all file i/o, encryption/decryption, clipboard operations, and system dialogs.
- renderer process: runs the user interface with full context isolation enabled, ensuring the renderer cannot directly access sensitive operations.

this architecture prevents the ui layer from directly accessing sensitive data or performing privileged operations, reducing the attack surface significantly.

security model

your vault is encrypted with aes-256-gcm using a cryptographic key derived from your master password via pbkdf2-sha512 with 250,000 iterations. all secrets (2fa seeds, passwords, notes) only exist in memory while the vault is unlocked. when you lock the app, all unencrypted data is wiped from memory. the application does not persist secrets to disk in plain text under any circumstances.

---

features

totp 2fa code generation

generate time-based one-time passwords using hmac with sha1, sha256, or sha512 algorithms. supports both 6-digit and 8-digit codes with configurable time periods (standard is 30 seconds). each code displays a live countdown timer showing when it will expire. right-click any code to copy it to your clipboard instantly.

account search and filter

search and filter all your 2fa accounts by issuer name or username using a live search bar at the top of the codes tab. results update as you type, making it quick to find the account you need.

password generator

generate strong, random passwords with customizable options:
- length: 8 to 64 characters
- character sets: uppercase, lowercase, numbers, symbols
- option to exclude ambiguous characters (0/o, 1/l/i, etc.)
- real-time strength meter showing password quality
- one-click copy to clipboard

secure notes

store sensitive text alongside your 2fa data. full create, read, update, and delete operations with the same aes-256-gcm encryption protecting your entire vault.

qr code import

scan 2fa qr codes from image files (png, jpeg formats). select an image file, and the app automatically decodes it using jsqr to extract account details. perfect for migrating from other authenticator apps or setting up new accounts.

import and export

import 2fa accounts from multiple formats:
- otpauth:// uris (standard totp uri format)
- google authenticator migration format
- json files
- encrypted .s2fa vault files (from other simpl2fa installations)

export your vault as either an encrypted backup file or plain text for migration to other authenticator applications.

auto-lock

your vault automatically locks after a configurable period of inactivity (1 minute to 1 hour). the inactivity timer resets whenever you move your mouse or press a keyboard key, so you won't be locked out during active use.

master password

a single master password unlocks your entire vault and all its contents. you can change your master password at any time from the settings panel. lock the app manually when stepping away from your computer.

backup reminders

the settings panel displays the date of your last export, helping you remember when your vault was last backed up. regular backups are recommended to prevent data loss.

---

application information

| property | value |
|----------|-------|
| version | 0.0.4 |
| build | beta 0.0.4 |
| runtime | electron 33 + chromium |
| platform | windows 64-bit |
| encryption standard | aes-256-gcm + pbkdf2-sha512 (250,000 iterations) |
| 2fa protocol | totp (rfc 6238) with sha1/sha256/sha512 support |
| code formats supported | 6-digit and 8-digit totp codes |
| qr decoder | jimp + jsqr (pure javascript, multi-strategy decoding) |
| import formats | otpauth:// uri, google authenticator, json, .s2fa |
| ui style | glassmorphism design |
| author | ivymroow |

---

getting started

1. download the latest release for your platform (windows 64-bit)
2. install and launch simpl2fa
3. create a strong master password on first run
4. import your 2fa accounts via qr code, image file, or plain text uri
5. use the app to generate codes whenever you need to authenticate

---

no cloud, no tracking

all your data stays on your computer. simpl2fa does not sync to the cloud, does not phone home, and does not track your usage. your vault is entirely self-contained and requires no internet connection to function.