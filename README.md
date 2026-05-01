# TIDAL DOWNLOADER

A high-fidelity desktop client for Tidal that downloads, plays, and organizes lossless music. Built with Electron + React.

[![Release](https://img.shields.io/github/v/release/ARCLIGHTSTRVL/tidal-downloader?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ARCLIGHTSTRVL/tidal-downloader/total?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-lightgrey?style=flat-square)]()
[![License](https://img.shields.io/badge/license-Proprietary-blue?style=flat-square)](LICENSE)

> **Status:** v1.0.2 — stable on Windows. macOS build available unsigned.

---

## Features

- **Lossless streaming & download** — FLAC at native sample rate, no transcoding for HiFi/Max tiers
- **Max-quality DASH support** — segment assembly + ffmpeg remux for HI_RES_LOSSLESS (24-bit / 96 kHz / 192 kHz)
- **Bit-perfect output (Windows)** — true WASAPI exclusive mode with native sample-rate negotiation, force-volume option
- **Library** — auto-scanned `Artist > Album` tree with list and grid views, library-wide playback, current-track highlight
- **Tag editor** — bulk album-level edits, embedded album art, drag-drop file/folder import, multi-root refresh
- **Search & discovery** — artist/album search with discography (Albums / EP & Singles), favorites, recent history sections
- **Playback** — gapless local playback via custom `local://` protocol, shuffle/repeat (off → one → album), responsive seek scrubber
- **Album art** — selectable embed quality (320 / 640 / 1280), hover tilt, full-resolution lightbox, separate art download path
- **Persistent state** — library index with Tidal canonical IDs (handles same-name artist collision like *LiSA* vs *LISA*)
- **History navigation** — mouse thumb buttons (XButton1 / XButton2) for app-wide back/forward across pages
- **Probe available quality** — quick check whether your subscription tier actually returns lossless or AAC for sample tracks (Settings → Check available quality)
- **Library maintenance** — resync metadata + reorganize files from Tidal online, or rebuild the library index offline from `TIDAL_GUID` / `TIDAL_META` Vorbis comments embedded in your FLACs

## Download

Latest release: **v1.0.2** — see [Releases](../../releases/latest).

| Platform | File | Size |
|----------|------|------|
| Windows 10/11 (x64) | `TIDAL DOWNLOADER Setup 1.0.2.exe` | ~118 MB |
| macOS Intel (x64) | `TIDAL DOWNLOADER-1.0.2.dmg` | ~200 MB |
| macOS Apple Silicon (arm64) | `TIDAL DOWNLOADER-1.0.2-arm64.dmg` | ~200 MB |

## Installation

### Windows
1. Download `TIDAL DOWNLOADER Setup 1.0.2.exe` from the latest release.
2. Run the installer — it is signed by **ARCLIGHTSTRVL** (Authenticode), so SmartScreen should not flag it.
3. Follow the wizard. Per-user install, no admin required.

### macOS
1. Download the `.dmg` matching your CPU (Intel or Apple Silicon).
2. Mount it and drag *TIDAL DOWNLOADER* into `/Applications`.
3. First launch: right-click → **Open** to bypass the unidentified-developer warning (current builds are unsigned).
4. Optionally clear the quarantine flag: `xattr -cr "/Applications/TIDAL DOWNLOADER.app"`.

## Requirements

- **Tidal subscription** — HiFi or HiFi Plus required for lossless / Max-quality streams.
- **Windows 10/11 (x64)** or **macOS 10.15+** (Intel or Apple Silicon).
- ~250 MB free disk space (excluding your music library).

## Quick start

1. Launch the app and sign in via the Tidal Device Code flow (your default browser opens automatically with a short code).
2. Set your download folder in **Settings → Download location**. The album-art folder is initialized to `<downloadPath>/art` automatically.
3. Search for any artist or album, then click **Download** on a track or **Download All** on an album.
4. Use the **Library** tab to play your downloaded collection — list mode for browsing, grid mode with artist avatars for visual scanning.
5. Use the **Tag Editor** for bulk metadata cleanup before archiving or sharing.

## Audio quality

Downloads are written as standard FLAC (no MP4 wrapper). When the Tidal manifest is DASH (HI_RES_LOSSLESS), the app assembles segments and remuxes losslessly via ffmpeg (`-c:a copy`). When the requested quality is unavailable for a track, the app falls back gracefully and only saves real FLAC — never a re-encoded AAC.

On Windows, **Use exclusive mode** in the audio device picker takes hold of the device for bit-perfect output and matches the source sample rate / bit depth (16/44.1, 24/96, 24/192). On macOS the toggle currently has no effect (Core Audio Hog Mode is on the roadmap).

## Reporting issues

Please open a bug or feature request on the [Issues page](../../issues).

When reporting a bug, please include:
- App version (**Settings → About** footer)
- OS + version
- Steps to reproduce
- Tidal subscription tier (HiFi / HiFi Plus)
- Console output if reproducible — on Windows, launch from PowerShell with:
  ```powershell
  $env:ELECTRON_ENABLE_LOGGING=1
  & "$env:LOCALAPPDATA\Programs\tidal-downloader\TIDAL DOWNLOADER.exe"
  ```

## Disclaimer

This is an unofficial, third-party tool. It is **not affiliated with, sponsored by, or endorsed by** Tidal or Aspiro AB.

You are responsible for complying with Tidal's Terms of Service. Downloads are intended for personal, offline access to music you have already paid for through your subscription. **Do not redistribute downloaded content.**

ffmpeg is used internally and is licensed under LGPL-2.1+ — see [ffmpeg.org/legal.html](https://ffmpeg.org/legal.html) for details. The bundled binaries come from [`ffmpeg-static`](https://www.npmjs.com/package/ffmpeg-static).

## License

Copyright © 2026 **ARCLIGHTSTRVL**. All rights reserved.

The compiled application is provided as-is for personal use. Source code is not publicly available. See [LICENSE](LICENSE) for the full terms.

---

Built by **ARCLIGHTSTRVL**.
