<div align="center">

<img src="assets/icon.svg" alt="TIDAL DOWNLOADER" width="160" />

# TIDAL DOWNLOADER

**English** | [한국어](README.ko.md)

A high-fidelity desktop client for Tidal — download lossless **FLAC** (16-bit / 24-bit up to 192 kHz HI_RES_LOSSLESS), grab whole **playlists**, play **bit-perfect** via WASAPI exclusive mode (Windows) or Core Audio Hog Mode (macOS), organize your library, and edit tags in bulk.<br>Built with Electron + React for Windows and macOS.

[![Release](https://img.shields.io/github/v/release/ARCLIGHTSTRVL/tidal-downloader?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ARCLIGHTSTRVL/tidal-downloader/total?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-lightgrey?style=flat-square)]()
[![License](https://img.shields.io/badge/license-Proprietary-blue?style=flat-square)](LICENSE)

</div>

> **Status:** v1.0.3 — stable on **Windows and macOS**. This release brings native macOS builds (Apple Silicon + Intel) with bit-perfect Core Audio Hog Mode, the full playlist system, English/한국어 UI, update notifications, and a deep file-safety hardening pass. Feedback welcome on [Issues](../../issues).

---

## Screenshots

<table>
  <tr>
    <td><img src="docs/images/library.png" alt="Library — artist and album grid with playlists" /></td>
    <td><img src="docs/images/library-list.png" alt="Library list view with per-track quality (FLAC 96 kHz / 24-bit)" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/search.png" alt="Search results — albums, playlists, and tracks" /></td>
    <td><img src="docs/images/search-home.png" alt="Search home with library stats and favorites" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/album-download.png" alt="Album download in progress — three tracks in parallel" /></td>
    <td><img src="docs/images/now-playing.png" alt="Now Playing view with ambient album backdrop" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/album-info.png" alt="Album info overlay with Tidal metadata and track list" /></td>
    <td><img src="docs/images/album-art.png" alt="Full-resolution album art lightbox" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/tag-editor.png" alt="Bulk tag editor with file info" /></td>
    <td><img src="docs/images/exclusive-mode.png" alt="Per-device exclusive mode (bit-perfect) options" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/settings-korean.png" alt="Settings in Korean (English/한국어 UI)" /></td>
    <td></td>
  </tr>
</table>

## How it works

- Tidal FLAC streams are saved as standard FLAC — no re-encode, no MP4 wrapper. HI_RES_LOSSLESS tracks come through as 24-bit at 96 or 192 kHz.
- DASH manifests (used for HI_RES_LOSSLESS) are reassembled and remuxed losslessly via ffmpeg (`-c:a copy`).
- Playback is bit-perfect on both platforms: **WASAPI exclusive mode** on Windows, **Core Audio Hog Mode** on macOS — both take the device exclusively and switch it to the source's native sample rate.
- Every download embeds Tidal-native identity inside the audio container itself (see below); the library index can be rebuilt offline from these tags alone.
- The Windows installer is Authenticode-signed by ARCLIGHTSTRVL.

## Built-in track identity

What makes this app different from a plain downloader: **every file it saves carries its own Tidal identity inside the audio container** — a unique `TIDAL_GUID` plus a `TIDAL_META` record (Tidal track ID, original title/artist/album) written as Vorbis comments in FLAC and iTunes boxes in M4A. Because the identity lives in the file, not in a database:

- **The app always recognizes its own downloads.** Rename the file, retag it, move it to another folder — the downloaded-✓ mark, album grouping, and duplicate detection stay accurate, verified against the embedded identity rather than file names or titles.
- **Your library survives anything.** Wipe the app, move your music to a new machine, or lose the library index entirely — one offline **Rebuild** reconstructs the whole index from the tags in your files, no internet needed.
- **No imposters.** A same-title file from somewhere else cannot masquerade as your verified download — files without a matching embedded identity are never treated as one.
- **Standard tags, zero lock-in.** They're ordinary metadata fields that every tagging tool can read (and remove, if you ever want to) — your files stay plain FLAC/M4A that play anywhere.

## Features

- **Lossless streaming & download** — FLAC at native sample rate, no transcoding for HiFi/Max tiers
- **Max-quality DASH support** — segment assembly + ffmpeg remux for HI_RES_LOSSLESS (24-bit / 96 kHz / 192 kHz)
- **Playlists** — browse Tidal playlists (search results, your own + favorites, recent), open any playlist by pasting its link or UUID, batch-download into a dedicated `playlists/<name>/` folder with playlist track order and cover art, and manage them as a first-class Library group
- **Bit-perfect output** — Windows: WASAPI exclusive mode with native sample-rate negotiation, force-volume option. macOS: Core Audio Hog Mode with nominal sample-rate matching
- **Fast album downloads** — album tracks download 3 at a time; Max-quality DASH segments are already parallel per track
- **Library** — auto-scanned `Artist > Album` tree with list and grid views, library-wide playback, current-track highlight, playlist-aware search
- **Tag editor** — bulk album-level edits, embedded album art, drag-drop file/folder import, multi-root refresh
- **Search & discovery** — artist/album search with discography (Albums / EP & Singles), favorites, recent history sections, library stats on the search home
- **Playback** — gapless local playback via custom `local://` protocol, shuffle/repeat (off → one → album), responsive seek scrubber
- **Album art** — selectable embed quality (320 / 640 / 1280), hover tilt, full-resolution lightbox, separate art download path
- **Update notifications** (Windows + macOS) — background version checks with a toast that links to the newest release, plus a manual check in Settings
- **English / 한국어** — switch the UI language instantly in Settings
- **Persistent state** — library index with Tidal canonical IDs (handles same-name artist collision like *LiSA* vs *LISA*); downloaded-✓ marks verified by embedded identity, so retagged or renamed files keep their checkmark
- **History navigation** — mouse thumb buttons (XButton1 / XButton2) for app-wide back/forward across pages
- **Probe available quality** — quick check whether your subscription tier actually returns lossless or AAC for sample tracks (Settings → Check available quality)
- **Library maintenance** — resync metadata + reorganize files from Tidal online, or rebuild the library index offline from the identity tags embedded in your files (FLAC and M4A)

## Download

### v1.0.3 (Windows + macOS)

See [Releases](../../releases/latest).

| Platform | File | Size |
|----------|------|------|
| Windows 10/11 (x64) | `TIDAL DOWNLOADER Setup 1.0.3.exe` | ~119 MB |
| macOS Apple Silicon (arm64) | `TIDAL DOWNLOADER-1.0.3-arm64.dmg` | ~129 MB |
| macOS Intel (x64) | `TIDAL DOWNLOADER-1.0.3.dmg` | ~141 MB |

> macOS builds are **unsigned** (Apple Developer notarization is on the roadmap) — see the install steps below for the one-time "Open Anyway" step.

## Installation

### Windows
1. Download `TIDAL DOWNLOADER Setup 1.0.3.exe` from the latest release.
2. Run the installer — it is signed by **ARCLIGHTSTRVL** (Authenticode), so SmartScreen should not flag it.
3. Follow the wizard. Per-user install, no admin required.

### macOS
1. Download the `.dmg` matching your CPU (Apple Silicon `arm64` or Intel `x64`).
2. Mount it and drag *TIDAL DOWNLOADER* into `/Applications`.
3. First launch: if macOS blocks the app, open **System Settings → Privacy & Security** and click **"Open Anyway"** (needed once). On older macOS versions, right-click → **Open** works too.
4. Alternatively, clear the quarantine flag from Terminal: `xattr -cr "/Applications/TIDAL DOWNLOADER.app"`.

## Requirements

- **Tidal subscription** — HiFi or HiFi Plus required for lossless / Max-quality streams.
- **Windows 10/11 (x64)** or **macOS 10.15+** (Intel or Apple Silicon).
- ~250 MB free disk space (excluding your music library).

## Quick start

1. Launch the app and sign in via the Tidal Device Code flow (your default browser opens automatically with a short code).
2. Set your download folder in **Settings → Download location**. The album-art folder is initialized to `<downloadPath>/art` automatically.
3. Search for any artist or album — or paste a playlist link — then click **Download** on a track, **Download All** on an album, or download the whole playlist at once.
4. Use the **Library** tab to play your downloaded collection — list mode for browsing, grid mode with artist avatars for visual scanning, and a dedicated Playlists group.
5. Use the **Tag Editor** for bulk metadata cleanup before archiving or sharing.

## Audio quality

Downloads are written as standard FLAC (no MP4 wrapper). When the Tidal manifest is DASH (HI_RES_LOSSLESS), the app assembles segments and remuxes losslessly via ffmpeg (`-c:a copy`). When the requested quality is unavailable for a track, the app falls back gracefully and only saves real FLAC — never a re-encoded AAC.

**Use exclusive mode** in the audio device picker takes hold of the device for bit-perfect output and matches the source sample rate / bit depth (16/44.1, 24/96, 24/192) — via WASAPI exclusive mode on Windows and Core Audio Hog Mode on macOS.

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

## Support

If you find TIDAL DOWNLOADER useful, you can support development on [Ko-fi](https://ko-fi.com/arclights). Every contribution helps keep the project maintained — thank you.

[![Ko-fi](https://img.shields.io/badge/Support_on-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/arclights)

---

Built by **ARCLIGHTSTRVL**.
