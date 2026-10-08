# Studio — releases

Installers and update metadata for Studio. **No source code lives here.**

This repository exists because Studio's source is private and GitHub release asset
visibility follows repository visibility, with no per-release override. Publishing
installers from the private repo would mean every update check needed a token with
read access to the source — and that token would have to ship inside the installer,
in every copy. So artefacts are published here instead, and the source stays private.

`electron-updater` inside the app reads the latest release from this repository.
Nothing here is written by hand: `.github/workflows/release.yml` in the source repo
uploads on a version tag.

## What a release contains

| File | What it is |
| --- | --- |
| `Studio-<version>-arm64.dmg` | macOS installer, Apple Silicon |
| `Studio-<version>.dmg` | macOS installer, Intel |
| `Studio-Setup-<version>.exe` | Windows installer |
| `Studio-<version>-*-mac.zip` | what the macOS updater downloads |
| `latest-mac.yml`, `latest.yml` | update metadata — the app reads these |

## Verifying a download

macOS builds are signed with a Developer ID certificate and notarised by Apple. To
check a copy you have:

```
codesign --verify --deep --strict --verbose=2 /Applications/Studio.app
spctl -a -vvv -t install /Applications/Studio.app
```

## Licences

Studio bundles FFmpeg under the LGPL v2.1. Its source, and the configuration used to
build it, are available from the FFmpeg project; the bundled build carries no
GPL-licensed components. Third-party notices ship inside the app.
