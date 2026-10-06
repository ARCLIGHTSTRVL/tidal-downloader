<div align="center">

<img src="assets/icon.svg" alt="TIDAL DOWNLOADER" width="160" />

# TIDAL DOWNLOADER

**English** | [한국어](README.ko.md)

Download music from Tidal, play your collection, and organize folders and tags.<br>For Windows and macOS.

[![Release](https://img.shields.io/github/v/release/ARCLIGHTSTRVL/tidal-downloader?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ARCLIGHTSTRVL/tidal-downloader/total?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-lightgrey?style=flat-square)
[![License](https://img.shields.io/badge/license-Proprietary-blue?style=flat-square)](LICENSE)

## Download

### v1.0.6 (Windows + macOS)

This version fixes the extra console window during Windows playback and makes the Tag Editor's album sort menu follow the selected language. See the [release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.6) or [changelog](CHANGELOG.md) for details.

<table align="center">
  <thead>
    <tr>
      <th align="center">Platform</th>
      <th align="center">Download</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">Windows 10/11 (x64)</td>
      <td align="center"><a href="https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.6/TIDAL-DOWNLOADER-Setup-1.0.6.exe">Setup .exe</a></td>
    </tr>
    <tr>
      <td align="center">macOS 12+ Apple Silicon (arm64)</td>
      <td align="center"><a href="https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.6/TIDAL-DOWNLOADER-1.0.6-arm64.dmg">DMG</a> · <a href="https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.6/TIDAL-DOWNLOADER-1.0.6-arm64-mac.zip">ZIP</a></td>
    </tr>
    <tr>
      <td align="center">macOS 12+ Intel (x64)</td>
      <td align="center"><a href="https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.6/TIDAL-DOWNLOADER-1.0.6.dmg">DMG</a> · <a href="https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.6/TIDAL-DOWNLOADER-1.0.6-mac.zip">ZIP</a></td>
    </tr>
  </tbody>
</table>

The Intel build was tested under Rosetta on Apple Silicon, not on Intel Mac hardware.

</div>

## Screenshots

Some screenshots show an earlier version of Settings. See the [User Guide](docs/USER_GUIDE.md#download-naming) for the current naming controls.

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

## Requirements

- Windows 10/11 x64 or macOS 12 Monterey or later.
- An active Tidal subscription. Available audio quality varies by account, region and track.
- Space for the app, your music and temporary downloads.

## Installation

### Windows

1. Download and run the Setup `.exe` above.
2. Follow the installer to choose a location. A per-user installation normally needs no administrator rights; all-user installations or protected existing locations may require them.

The installer is self-signed by ARCLIGHTSTRVL and timestamped. Windows may show a trust or SmartScreen warning.

### macOS

1. Download the DMG for your Mac: `arm64` for Apple Silicon, `x64` for Intel.
2. Open the DMG and drag **TIDAL DOWNLOADER** into **Applications**. If you are updating, replace the existing app after quitting it.
3. Eject the DMG and open the app from Applications. For the ZIP download, extract it and move the app to Applications.

Mac builds have no Apple Developer ID signature or notarization, so Gatekeeper blocks them by default. To allow the app, use **Open Anyway** under **System Settings → Privacy & Security**. On Monterey, this is under **System Preferences → Security & Privacy**.

### Updating from an earlier version

Use the downloads above to update manually. Quit the app first, including any tray or menu-bar instance. Automatic installation is not reliable with the current signing setup.

Existing valid saved settings, including download paths and naming rules, are retained. You do not need to reset settings. Updating the app does not reorganize your music files.

## Quick start

1. Open the app, click **Login**, and complete sign-in in the Tidal window.
2. Choose a folder in **Settings → Download location**.
3. Choose your audio quality and set **Download naming** before your first download. Edit the folder and filename rules directly or with tags, check the preview, then select **Save**.
4. Search for an artist or album, or paste a playlist link. Use a track's download icon to save one song, or the download icon below the album information to save the album.
5. Play your downloaded music from **Library**. Use **Tag Editor** to edit metadata and album art.

New settings use `Album artist/Album` folders and `Track number - Title` filenames. Existing valid rules stay in place. Saved naming changes apply to future downloads. To group existing files by album type, use **Group existing library by album type → Preview**, review the proposed paths, then select **Apply**.

The [User Guide](docs/USER_GUIDE.md) covers playlists, naming examples and library maintenance in more detail.

## Features

- Search artists, albums and playlists; save individual tracks or whole collections.
- Edit folder and filename rules with tags or direct text, with presets and sample previews.
- Optionally organize releases into `Albums`, `EPs`, `Singles` and `Compilations` folders.
- Browse the library in list or grid view, sort albums, and manage playlists.
- Edit tags and album art for multiple tracks at once.
- Play local music with shuffle, repeat and a choice of audio output device.
- Switch between English and Korean in Settings.

## Audio quality

| Setting | Requested quality |
|---------|-------------------|
| Max | FLAC up to 24-bit / 192 kHz |
| HiFi | FLAC at 16-bit / 44.1 kHz |
| High | AAC at 320 kbps |

If lossless audio is unavailable, **Max and HiFi can fall back to High and save AAC as `.m4a`**. Check the quality shown for the track. **Settings → Check available quality** samples what Tidal currently returns for your account.

<a id="how-it-works"></a>

FLAC streams are saved as `.flac`, and AAC streams as `.m4a`. Downloaded audio is copied without re-encoding.

### Audio output

**Use exclusive mode** requests WASAPI exclusive output on Windows or Core Audio Hog Mode on macOS. The output format depends on the device, and playback can fall back to shared output if exclusive playback fails. Volume adjustments also change the audio, so enabling exclusive mode alone does not guarantee bit-perfect playback.

## Built-in track identity

The app stores Tidal identifiers in supported FLAC and M4A files using `TIDAL_GUID` and `TIDAL_META`. Keep these fields when using another tag editor: they help the app recognize renamed files and rebuild the library index offline.

After moving files outside the app, refresh or rebuild the library at its current location. Missing or damaged identity tags may prevent recovery of some entries. Some older library entries can match by title, so a download checkmark alone is not proof of a file's identity. If tags or the library index could not be saved, check the result in **Downloads**.

## Reporting issues

Open an [issue](../../issues) with the app version shown at the bottom of Settings, your OS version, and the steps to reproduce the problem. For audio problems, include the requested quality and output device.

<details>
<summary>Collecting logs on Windows</summary>

Quit the app, then run it from PowerShell:

```powershell
$env:ELECTRON_ENABLE_LOGGING=1
& "$env:LOCALAPPDATA\Programs\TIDAL DOWNLOADER\TIDAL DOWNLOADER.exe"
```

This is the default per-user path. Use the actual location if you chose another folder, installed for all users, or kept a different path from an older installation.

</details>

## Disclaimer

This is an unofficial app, not affiliated with, sponsored by, or endorsed by Tidal or Aspiro AB. Use it in accordance with Tidal's Terms of Service, for personal offline listening. Do not redistribute downloaded content.

## License

Copyright © 2026 **ARCLIGHTSTRVL**. All rights reserved. The compiled app is provided as-is for personal use; its source code is private. See [LICENSE](LICENSE) for the terms.

FFmpeg is included under its applicable LGPL or GPL terms; the bundled Windows build enables GPL components. See [FFmpeg's license information](https://ffmpeg.org/legal.html), [source downloads](https://ffmpeg.org/download.html), and the [`ffmpeg-static` release and license files](https://github.com/eugeneware/ffmpeg-static/releases/tag/b6.1.1).

## Support

You can support the project with a [GitHub star](https://github.com/ARCLIGHTSTRVL/tidal-downloader) or a [Ko-fi contribution](https://ko-fi.com/arclights). The app also has a **GitHub Star** link below **Check for updates** in Settings.

[![Ko-fi](https://img.shields.io/badge/Support_on-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/arclights)
