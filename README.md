# thaleon

Landing page + desktop releases for **thaleon-agent** — a chat-first desktop
copilot for Malaysian Unit Trust Consultants.

- **Landing page**: published via GitHub Pages (`index.html` at repo root) →
  https://itechchoice.github.io/thaleon/
- **Downloads**: latest builds are attached to the newest
  [GitHub Release](https://github.com/itechchoice/thaleon/releases/latest).

| OS | Asset | Requirements | Signed? |
|---|---|---|---|
| macOS | `thaleon-agent.dmg` | macOS 12+ · Apple Silicon (M1 or newer) | ✅ Developer ID + Apple notarized |
| Windows | `thaleon-agent-setup.exe` | Windows 10 / 11 · x64 | ⚠️ Unsigned — SmartScreen "Unknown publisher" warning on first launch |

Stable latest-release URLs (never change across versions — the landing page
and electron-updater both hard-code these):

```
https://github.com/itechchoice/thaleon/releases/latest/download/thaleon-agent.dmg
https://github.com/itechchoice/thaleon/releases/latest/download/thaleon-agent-setup.exe
```

## Release flow

This repo is **asset-only** — the source code lives in the private
`itechchoice/thaleon-agent-desktop` repo. Releases are cut from there via
`pnpm release "notes"`:

1. macOS dmg is built, signed, notarized, and stapled locally on the
   maintainer's Mac, then uploaded here via `gh release create`.
2. The same `release.sh` tags the source repo and triggers a GitHub
   Actions `windows-latest` workflow that builds the NSIS installer and
   uploads the `.exe` + `latest.yml` + blockmap to the same release.
3. Auto-update is driven by `latest-mac.yml` (Mac) and `latest.yml`
   (Windows) at the root of each release.

Code-signing the Windows build is on the roadmap. When a certificate is
purchased, set `WINDOWS_CERT_FILE_BASE64` + `WINDOWS_CERT_PASSWORD` as
repo secrets on the source repo — the workflow picks them up
automatically (the slot is already wired in `electron-builder.yml`).
