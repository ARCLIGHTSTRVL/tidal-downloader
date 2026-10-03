<div align="center">

<img src="assets/icon.svg" alt="TIDAL DOWNLOADER" width="160" />

# TIDAL DOWNLOADER

**English** | [한국어](README.ko.md)

A high-fidelity desktop client for Tidal — download lossless **FLAC** (16-bit / 24-bit up to 192 kHz HI_RES_LOSSLESS), grab whole **playlists**, play **bit-perfect** via WASAPI exclusive mode (Windows) or Core Audio Hog Mode (macOS), organize your library, and edit tags in bulk.<br>Built with Electron + React for Windows and macOS.

[![Release](https://img.shields.io/github/v/release/ARCLIGHTSTRVL/tidal-downloader?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ARCLIGHTSTRVL/tidal-downloader/total?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-lightgrey?style=flat-square)
[![License](https://img.shields.io/badge/license-Proprietary-blue?style=flat-square)](LICENSE)

</div>

> **Latest: v1.0.5** — clearer folder and file naming, optional album-type folders, improved library sorting, and better feedback when saving settings or working with files. Available for **Windows x64 and macOS Apple Silicon / Intel**. See the [changelog](CHANGELOG.md) and [release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.5).

---

## Screenshots

Some screenshots show an earlier version of Settings. See the [User Guide](docs/USER_GUIDE.md#download) for the current naming controls.

<table>
  <tr>
    <td><img src="docs/images/library.png" alt="Library — artist and album grid with playlists" /></td>
    <td><img src="docs/images/library-list.png" alt="Library list view with per-track quality (FLAC 96 kHz / 24-bit)" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/search-home.png" alt="Search home with library stats and favorites" /></td>
    <td><img src="docs/images/search.png" alt="Search results — albums, playlists, and tracks" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/album-download.png" alt="Album download in progress — three tracks in parallel" /></td>
    <td><img src="docs/images/album-art.png" alt="Full-resolution album art lightbox" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/now-playing.png" alt="Now Playing view with ambient album backdrop" /></td>
    <td><img src="docs/images/album-info.png" alt="Album info overlay with Tidal metadata and track list" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/exclusive-mode.png" alt="Per-device exclusive mode (bit-perfect) options" /></td>
    <td><img src="docs/images/tag-editor.png" alt="Bulk tag editor with file info" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/downloads.png" alt="Downloads panel — active progress and completed tracks" /></td>
    <td><img src="docs/images/settings-korean.png" alt="Settings in Korean (English/한국어 UI)" /></td>
  </tr>
</table>

## How it works

- Tidal FLAC streams are saved as standard FLAC without re-encoding the audio. Max requests lossless quality up to 24-bit / 192 kHz, depending on the track and Tidal's response.
- DASH manifests (used for HI_RES_LOSSLESS) are reassembled and remuxed losslessly via ffmpeg (`-c:a copy`).
- Exclusive playback uses **WASAPI exclusive mode** on Windows or **Core Audio Hog Mode** on macOS and requests the source's sample rate. Available formats and exclusive access depend on the audio device.
- The app writes Tidal identity into supported FLAC/M4A downloads (see below). These tags support offline index recovery; incomplete tag or index writes are reported in Downloads.
- The Windows installer is self-signed by ARCLIGHTSTRVL and timestamped. Windows may still show a trust or SmartScreen warning.

## Built-in track identity

The app stores **Tidal identity inside supported audio files** — a unique `TIDAL_GUID` plus a `TIDAL_META` record (Tidal track ID, original title/artist/album) written as Vorbis comments in FLAC and iTunes boxes in M4A. Files with these fields can be matched independently of their names:

- **Match downloads after renaming or retagging.** Retained embedded identity can help the app recognize a file despite changed names or tags. After moving files outside the app, refresh or rebuild the library in its current location.
- **Recover the library index offline.** **Rebuild** reads the identity tags in accessible FLAC and M4A files. Keep these fields intact when using another tag editor; files with missing or damaged identity may need attention.
- **Read checkmarks in context.** Some older library entries can match by title when embedded identity is not required. A download checkmark alone is not proof of a file's identity.
- **Standard tags, zero lock-in.** They're ordinary metadata fields that every tagging tool can read (and remove, if you ever want to) — your files stay plain FLAC/M4A that play anywhere.

## Features

- **Every Tidal quality tier** — Max (HI_RES_LOSSLESS 24-bit FLAC up to 192 kHz), HiFi (16/44.1 FLAC), and High (AAC 320 kbps as proper `.m4a`) — all with the same embedded identity and library features, never transcoded
- **Max-quality DASH support** — segment assembly + ffmpeg remux for HI_RES_LOSSLESS (24-bit / 96 kHz / 192 kHz)
- **Playlists** — browse Tidal playlists (search results, your own + favorites, recent), open any playlist by pasting its link or UUID, batch-download into a dedicated `playlists/<name>/` folder with playlist track order and cover art, and manage them as a first-class Library group
- **Bit-perfect output** — Windows: WASAPI exclusive mode with native sample-rate negotiation, force-volume option. macOS: Core Audio Hog Mode with nominal sample-rate matching
- **Fast album downloads** — album tracks download 3 at a time; Max-quality DASH segments are already parallel per track
- **Library** — list and grid views, album sorting by title, year or recently added, library-wide playback, current-track highlight, playlist-aware search
- **Folder and file naming** — edit the same rule directly or with tags, use presets and sample previews, then save explicitly. New settings default to `Album artist/Album` folders and `Track number - Title` filenames; existing valid saved rules stay in place
- **Album-type folders** — optionally group releases into `Albums`, `EPs`, `Singles` and `Compilations`. Preview existing-library changes and select **Apply** when ready
- **Tag editor** — bulk album-level edits, embedded album art, drag-drop file/folder import, multi-root refresh
- **Search & discovery** — artist/album search with discography (Albums / EP & Singles), favorites, recent history sections, library stats on the search home
- **Playback** — local playback via custom `local://` protocol, shuffle/repeat (off → one → album), responsive seek scrubber
- **Album art** — selectable embed quality (320 / 640 / 1280), hover tilt, full-resolution lightbox, separate art download path
- **Update checks** (Windows + macOS) — background version checks and a manual check in Settings. Use the release files below for manual updates with the current signing setup
- **English / 한국어** — switch the UI language instantly in Settings
- **Persistent state** — a library index stores Tidal canonical IDs to distinguish artists such as *LiSA* and *LISA*. File matching uses embedded identity where available, with title-based matching for some older entries
- **History navigation** — mouse thumb buttons (XButton1 / XButton2) for app-wide back/forward across pages
- **Probe available quality** — quick check whether your subscription tier actually returns lossless or AAC for sample tracks (Settings → Check available quality)
- **Library maintenance** — resync metadata + reorganize files from Tidal online, or rebuild the library index offline from the identity tags embedded in your files (FLAC and M4A)

## Download

### v1.0.5 (Windows + macOS)

See [Releases](../../releases/latest).

| Platform | Download |
|----------|----------|
| Windows 10/11 (x64) | [Setup .exe](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-Setup-1.0.5.exe) |
| macOS 12+ Apple Silicon (arm64) | [DMG](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-arm64.dmg) · [ZIP](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-arm64-mac.zip) |
| macOS 12+ Intel (x64) | [DMG](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5.dmg) · [ZIP](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-mac.zip) |

The Intel build was exercised under Rosetta on Apple Silicon, not on Intel Mac hardware. Mac builds are not signed with an Apple Developer ID or notarized; Gatekeeper blocks them by default.

## Installation

### Windows
1. Quit TIDAL DOWNLOADER and download `TIDAL-DOWNLOADER-Setup-1.0.5.exe` from the release above.
2. Run the installer. Its self-signed certificate is not rooted in Windows' trusted certificate store, so a trust or SmartScreen warning may appear.
3. Follow the wizard. A per-user installation normally does not require administrator rights. All-user installations or changes to a protected existing installation may prompt for elevation.

### macOS
1. Download the `.dmg` matching your CPU (Apple Silicon `arm64` or Intel `x64`).
2. Mount it and drag *TIDAL DOWNLOADER* into `/Applications`.
3. If macOS blocks the app, review **Privacy & Security** in System Settings (or **Security & Privacy** in System Preferences on Monterey) and use **Open Anyway** if you choose to allow it.

### Updating from an earlier version

Download the appropriate release file and update manually after quitting the app, including its tray or menu-bar instance. Automatic installation is not reliable with the current Windows and Mac signing setup.

Existing valid saved settings, including download paths and naming rules, are retained. The new default naming layout applies when no valid saved value exists; installing a new version does not itself reorganize your music. In Settings, save naming changes first, then preview and explicitly apply any existing-library changes.

## Requirements

- **An active Tidal subscription** with access to the requested audio quality. Availability can vary by account, region and track.
- **Windows 10/11 (x64)** or **macOS 12 Monterey or later** (Intel or Apple Silicon).
- Free disk space for the application, downloaded music and temporary downloads.

## Quick start

1. Launch the app, click **Login**, and complete sign-in in the Tidal sign-in window.
2. Set your download folder in **Settings → Download location**. The album-art folder is initialized to `<downloadPath>/art` automatically.
3. Search for any artist or album — or paste a playlist link — then click **Download** on a track, **Download All** on an album, or download the whole playlist at once.
4. Use the **Library** tab to play your downloaded collection — list mode for browsing, grid mode with artist avatars for visual scanning, and a dedicated Playlists group.
5. Use **Settings → Download naming** to choose your folder and file rules, then **Save**. Use the **Tag Editor** for bulk metadata edits.

## Audio quality

Choose **Max**, **HiFi** or **High** in Settings. Use **Check available quality** to see what Tidal currently returns for your account. High is AAC by design; AAC returned for a lossless request is reported separately.

Max and HiFi downloads are written as standard FLAC (no MP4 wrapper). When the Tidal manifest is DASH (HI_RES_LOSSLESS), the app assembles segments and remuxes losslessly via ffmpeg (`-c:a copy`). The High tier saves true AAC 320 kbps as `.m4a`. Nothing is ever transcoded or disguised: when a lossless tier is unavailable for a track, the app falls back gracefully and never passes re-encoded AAC off as FLAC.

**Use exclusive mode** in the audio device picker takes hold of the device for bit-perfect output and matches the source sample rate / bit depth (16/44.1, 24/96, 24/192) — via WASAPI exclusive mode on Windows and Core Audio Hog Mode on macOS.

## Reporting issues

Please open a bug or feature request on the [Issues page](../../issues).

When reporting a bug, please include:
- App version (at the bottom of **Settings**)
- OS + version
- Steps to reproduce
- Requested audio quality (Max / HiFi / High), and your subscription plan if relevant
- Console output if reproducible — on Windows, launch from PowerShell with:
  ```powershell
  $env:ELECTRON_ENABLE_LOGGING=1
  & "$env:LOCALAPPDATA\Programs\tidal-downloader\TIDAL DOWNLOADER.exe"
  ```

## Disclaimer

This is an unofficial, third-party tool. It is **not affiliated with, sponsored by, or endorsed by** Tidal or Aspiro AB.

You are responsible for complying with Tidal's Terms of Service. Downloads are intended for personal, offline access to music you have already paid for through your subscription. **Do not redistribute downloaded content.**

FFmpeg is used internally; its applicable LGPL or GPL terms depend on the binary's build options. The bundled Windows build enables GPL components. See [FFmpeg's legal information](https://ffmpeg.org/legal.html), [FFmpeg source downloads](https://ffmpeg.org/download.html), and the [`ffmpeg-static` binary release and license files](https://github.com/eugeneware/ffmpeg-static/releases/tag/b6.1.1).

## License

Copyright © 2026 **ARCLIGHTSTRVL**. All rights reserved.

The compiled application is provided as-is for personal use. Source code is not publicly available. See [LICENSE](LICENSE) for the full terms.

## Support

You can star the [GitHub repository](https://github.com/ARCLIGHTSTRVL/tidal-downloader) from here or from the **GitHub Star** link below **Check for updates** in Settings.

If you find TIDAL DOWNLOADER useful, you can support development on [Ko-fi](https://ko-fi.com/arclights). Every contribution helps keep the project maintained — thank you.

[![Ko-fi](https://img.shields.io/badge/Support_on-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/arclights)

---

Built by **ARCLIGHTSTRVL**.
