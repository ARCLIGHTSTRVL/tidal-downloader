# TIDAL DOWNLOADER — User Guide

This guide covers everything you need to use the app day-to-day. For installation see the [README](../README.md).

## Contents
- [First launch](#first-launch)
- [Audio quality](#audio-quality)
- [Album art quality](#album-art-quality)
- [Search & discovery](#search--discovery)
- [Downloading](#downloading)
- [Library](#library)
- [Downloads page](#downloads-page)
- [Tag editor](#tag-editor)
- [Player bar](#player-bar)
- [Audio device picker (Windows)](#audio-device-picker-windows)
- [Settings reference](#settings-reference)
  - [Account](#account)
  - [Playback](#playback)
  - [Audio quality](#audio-quality-1)
  - [Album art quality](#album-art-quality-1)
  - [Download](#download)
  - [Library maintenance](#library-maintenance)
  - [Reset](#reset)
- [Mouse & keyboard shortcuts](#mouse--keyboard-shortcuts)
- [Troubleshooting](#troubleshooting)

---

## First launch

1. The app opens with a Tidal sign-in modal. Click **Login**.
2. Your default browser opens with an 8-character code pre-filled (or paste it manually). Approve the device.
3. The app picks up the token automatically — the modal closes and the home view appears.
4. Open **Settings → Download location** and pick a folder. The album-art folder is initialized to `<downloadPath>/art` automatically; you can change it under **Album art download location**.

The app refreshes the Tidal token automatically every five minutes.

## Audio quality

Tidal serves audio in two manifest formats:

| Quality | Manifest | What you get |
|---------|----------|--------------|
| **Max** (HI_RES_LOSSLESS / HI_RES) | DASH | 24-bit FLAC at the album's native sample rate (typically 44.1 / 48 / 96 / 192 kHz). The app downloads the segments and remuxes them into a standard FLAC file losslessly (`-c:a copy`). |
| **HiFi** (LOSSLESS) | BTS (single URL) | 16-bit / 44.1 kHz FLAC, downloaded as one file. |
| **High** | BTS (single URL) | AAC. Skipped automatically if you ask for LOSSLESS or higher. |

**Settings → Audio quality** controls both playback and download. Max takes ~1–3 s on first play because the app assembles DASH segments before sending to ffmpeg; HiFi plays instantly when Tidal serves it as 16-bit FLAC.

### Quality fallback when a track isn't available at the requested tier

Tidal does not always deliver the tier you asked for. The app handles this by stepping down through the lossless ladder:

- **Max requested → HI_RES_LOSSLESS / HI_RES delivered**: 24-bit FLAC at the album's native sample rate. ✅
- **Max requested → only LOSSLESS delivered**: the file is saved as 16-bit / 44.1 kHz FLAC (HiFi tier). The download still goes through, but at the lower native bit depth that Tidal made available. This is a real, lossless FLAC — just not 24-bit.
- **HiFi requested → only LOSSLESS delivered**: 16-bit FLAC, exactly as expected.
- **Either tier requested → only AAC available**: the app **does not save** an AAC pretending to be lossless. The track is skipped; only real FLAC reaches your library. (The library "✓" mark also won't appear, so you can re-attempt later from a different account or after a Tidal-side fix.)
- **High requested**: AAC is what you asked for; saved directly.

In short, you can't get a worse-than-FLAC file from a HiFi/Max download, but you may get FLAC at a lower bit depth than you wanted (silent downgrade by Tidal). Use **Settings → Check available quality** to find out in advance which tiers your account can actually fetch lossless audio at.

## Album art quality

**Settings → Album art quality** controls the resolution embedded into your downloaded files and shown in the lightbox.

- **High** — 1280 × 1280 (best, larger file)
- **Standard** — 640 × 640 (good balance)
- **Low** — 320 × 320 (small)

The app uses a fallback chain: if Tidal doesn't have the requested size for an album, it tries the next-smaller until one works. Embedded art is verified by JPEG/PNG/WebP magic bytes, so a stray HTML error response from the CDN never gets stamped into your tags.

## Search & discovery

The home view has four sections:

- **Favorites** — heart-marked artists (stored in `~/.tidal-downloader-favorites.json`)
- **Recent Artists**
- **Recent Albums**
- **Recent EP & Singles**

Each section auto-fits two rows; click **View all** when there's more.

The top search bar is sticky. Hit Enter to switch to the full search-results grid.

Click any artist to open their **Artist page** with a 180 × 180 circular avatar, a heart for favoriting, and split sections for *Albums* and *EP & Singles*. Clicking an artist's name from any other view (player bar, album detail, library grid) navigates here using the canonical Tidal artist ID — so *LiSA* and *LISA* never collide.

Click any album to open its **Album page** with a large cover (which you can hover-tilt and click for the full-resolution lightbox), the track list, and per-track download buttons.

## Downloading

- **Single track** — click the download icon next to a track.
- **Whole album** — click **Download All** on the album page. Tracks already downloaded (✓) are skipped automatically.
- **Stop** — a stop button appears next to the progress bar; cancels the album-level download or an individual track.
- **Progress** — the progress bar increases monotonically across an album, including resume scenarios. No flicker between tracks.

The first time you download from an artist whose folder name conflicts with an existing same-name artist, the app prompts you for an override (e.g. `LiSA (KR)`). The next download from that same canonical artist ID re-uses your override silently.

When you have **Album art quality = High** set, the embedded art is verified to be 1280 × 1280 (or whatever fallback succeeded) — check the Settings → About if you ever want to confirm what's currently in your tags.

## Library

The Library tab automatically scans your download folder using the `Artist > Album` structure.

Two view modes:
- **List** — Artist → Album → Track tree. Currently-playing track is highlighted in cyan.
- **Grid** — artist sections with circular avatars (auto-fetched from Tidal) and album-art cards. Click an album to open a 75 %-width detail view with the large cover and track list.

In Grid mode, each artist section header has an `✕` button (top-right). Clicking it asks for confirmation, then deletes the entire artist folder recursively and scrubs the library index.

The **Refresh** button (top-right) re-scans, then prompts you to delete any *truly empty* folders (no audio, no images, nothing). The first scan on app launch never prompts — only manual refreshes do.

The **Remux** button performs a one-shot conversion of any DASH-wrapped FLAC files in your library into standard FLAC (with tags + album art embedded). Useful for legacy downloads from earlier app versions.

You can play any local file by clicking it — the app uses a custom `local://` protocol that preserves the native bit depth and embedded album art.

## Downloads page

The **Downloads** tab in the sidebar (between **Tag Editor** and **Settings**) shows download activity at a glance.

- **In progress** — currently-downloading tracks with their album art, title, and a real-time progress bar.
- **Completed / failed** — finished history with a status icon (✓ for success, ✗ for failure with the reason on hover).
- **Click a row** — plays that track from Tidal (uses the streaming path, not the local file).
- **✕ on a row** — removes that entry from the list (the actual file on disk is **not** deleted).
- **Clear All** — wipes the entire history list.

The list is in-memory only — closing the app clears it. Persistent records of which tracks you have downloaded live in the library index (`<downloadFolder>/.tidal-library.json`), which is what powers the ✓ marks in Search and the Library tab.

## Tag editor

Open the Tag Editor tab; the app auto-loads your default download folder on first visit and tracks any additional roots you bring in (Open Folder, drag-drop).

- **Refresh** — re-scans all loaded roots
- **Open Files / Folder** — adds files or recursive folder contents
- **Clear** — opens a checkbox modal listing loaded roots; you choose which to remove from the editor (the disk files are not deleted)

Albums are shown as a grid (sortable by Artist / Album / Year / Recent). Click an album for the **album detail** view:
- Large cover top-left, with **Change Art** to swap from clipboard or local file
- Album-level fields (Album / Album Artist / Artist / Year / Genre / Composer) with **Apply to All N Tracks**
- Track list on the left, per-track metadata + file info on the right
- Drag-drop a new image directly onto the cover to update it

DASH-wrapped FLACs (rare unless you have legacy v1.0.0 downloads) save edits to a soft index; running **Remux** in the Library tab finalizes them into proper FLAC.

## Player bar

Slides up when you start playback. Layout:

```
[ shuffle ] ... [ prev ][ play ][ next ] ... [ repeat ]
       72 px fixed                     72 px fixed
        seek bar (responsive width: max(400px, 33vw))
       quality badge   download   volume   exclusive picker
```

- **Shuffle** — two states (off / album). When on, the next track within the current album/folder is randomized.
- **Repeat** — three states (off → one → album → off):
  - *one* — replay the current track on end
  - *album* — restart the album when the last track finishes
- **Seek** — pointer-event scrubber. Drag freely off the bar; release to commit. Hovering grows the bar from 4 → 8 px.
- **Quality badge** — Max (gold) / HiRes (warm gold) / HiFi (teal) / High (white) / Low (grey).
- **Album art click** — opens the album detail in the Search page.
- **Artist name click** — opens the artist page in Search.

When auto-advance falls past the album, the app continues sequentially through your library (artist → album → track number ordering).

## Audio device picker (Windows)

The speaker icon to the left of the quality badge opens a popover listing your system audio devices. Click one to route playback there (`setSinkId`). Click the gear icon next to the active device for **Device settings**:

- **Use exclusive mode** — grabs the device exclusively and outputs at the source's native sample rate / bit depth via WASAPI. While exclusive is on, other apps on that device produce no sound.
- **Force volume** — locks playback to 100 % so the slider stays out of the bit-perfect signal. Only available when exclusive mode is on.

Exclusive mode requires an app restart to take full effect (Electron's `commandLine` switch must be set before app.ready). Toggling it shows a one-shot toast.

> **macOS:** the toggles are visible but currently no-op — Core Audio "Hog Mode" support is on the roadmap.

## Settings reference

The Settings page groups options by purpose. Each section is described below.

### Account

- **Tidal** — Sign in or sign out via Tidal's OAuth Device Code flow. When signed in, the current account is shown. The browser opens automatically with your one-time code on each new login.
- **Auto-refresh** — When on, the app silently refreshes your Tidal access token in the background (every five minutes by default). Keep this on so you don't get logged out across launches; the `refresh_token` is preserved even when Tidal omits it from the response.

### Playback

- **Background playback & tray** — When on, closing the main window minimizes the app to the system tray instead of quitting. Right-click the tray icon to **Show TIDAL DOWNLOADER** or quit. Use this if you want the app to keep playing music while it's out of the way.

### Audio quality

This section sets the requested audio tier for **both playback and downloads**. The detailed timing characteristics are described in the [Audio quality](#audio-quality) section above.

- **Max** — 24-bit HiRes (DASH, remuxed to standard FLAC).
- **HiFi** — 16-bit / 44.1 kHz FLAC (BTS, single URL).
- **High** — AAC.

#### Check available quality

Runs a quick probe against Tidal to see which tiers actually return lossless audio for your account right now. The app fetches two short sample tracks (one from your library, one a global hit) at each tier and reports whether Tidal delivered FLAC or AAC.

This is the diagnostic counterpart to the [quality fallback](#quality-fallback-when-a-track-isnt-available-at-the-requested-tier) above. If a tier comes back as AAC in the probe, downloads at that tier will be **skipped** (the app refuses to save AAC pretending to be lossless). If a tier comes back as 16-bit FLAC when you asked for Max, downloads at that tier will save successfully but **silently downgraded** to 16 bit. Either way, knowing in advance lets you decide whether to lower the Audio quality setting (for fewer skips) or contact Tidal about your subscription state.

### Album art quality

Resolution of the album art embedded into downloaded files and shown in the lightbox. Falls back to the next smaller size automatically if Tidal doesn't have the requested resolution for an album.

- **High** — 1280 × 1280 (best, larger file)
- **Standard** — 640 × 640 (good balance)
- **Low** — 320 × 320 (small)

- **Album art hover tilt** — When on, album cards smoothly tilt toward the mouse cursor (3D parallax) across the Library, Search, and Tag Editor pages. Toggle off if you find it distracting; cards remain static.

### Download

- **Download location** — Root folder for downloaded audio. Setting this for the first time also initializes the album-art folder to `<downloadPath>/art`.
- **Album art download location** — Separate folder for full-resolution art saved from the lightbox. If unset, the lightbox download falls back to the audio folder.
- **Folder structure** — A chip-based builder that defines the subfolder tree under your download location. Click a tag to append (**Album Artist** / **Artist** / **Album** / **Genre** / **Year**), ✕ on the right to remove the last one. The live preview shows what the path will look like with sample data. Affects new downloads only; to re-apply your current rules to existing files, use **Library maintenance → Resync from TIDAL**.
- **File naming** — Same chip builder for the file name itself. Tags: **Album Artist**, **Artist**, **Track #**, **-** (separator), **Title**. The `.flac` extension is appended automatically.

### Library maintenance

Actions for keeping your downloaded library tidy. None of them delete audio files.

- **Resync from TIDAL (online)** — Re-fetches every downloaded track's metadata from Tidal and reapplies your current Folder structure + File naming rules. Tags and embedded album art are also refreshed. Requires internet. Use after changing your folder/naming pattern so the existing library matches.
- **Rebuild from FLAC (offline)** — Reads the `TIDAL_GUID` / `TIDAL_META` Vorbis comments that the app embeds into every downloaded FLAC and rebuilds the library index from them. No network required. Use this after moving files between drives, recovering from a corrupted `.tidal-library.json`, or transferring your library to another machine.

### Reset

Clear cached state without touching your audio files.

- **Reset library data** — Clears your favorites list and the Chromium HTTP cache (which holds Tidal CDN images) in one step. Use this when favorites need a fresh start or when artist profile photos / album art appear stale. Two-step confirmation: the first click turns the button red and waits up to 5 seconds for a second click to actually run.
- **Reset current settings** — Restores all Settings values to their defaults. Same two-step confirmation. Your downloaded files, library index, and Tidal token are not touched.

## Mouse & keyboard shortcuts

- **Mouse thumb buttons (XButton1 / XButton2)** — app-wide back / forward, including across pages (Library album detail ↔ Library grid, Search album ↔ artist page, Tag Editor album detail ↔ grid).
- **Sidebar re-click on the current page** — resets that page (Search → home; Library → list/grid root; Tag Editor → grid root). Clicking a different page tab navigates without resetting state.
- **Search bar Enter** — switches to the full results grid (from the dropdown overlay).
- **Spacebar** — play / pause (when no input field is focused).
- **Esc / background click** — closes most modals (lightbox, Audio Device settings, etc.).

## Troubleshooting

**The album art doesn't update after re-download.**
Open **Settings → Reset → Clear image cache** (2-step confirm). This empties Chromium's HTTP cache so the new image isn't masked by a cached older version.

**Library grid shows the wrong artist photo.**
Hit **Refresh** in the Library tab — the avatar cache is cleared on every refresh, so the next pass fetches fresh data via canonical artist IDs.

**Search results take me to the home view briefly before the album loads.**
This was a v1.0.1 bug. Update to v1.0.2 or later.

**Exclusive mode does not engage / volume slider doesn't move (Windows).**
Exclusive mode requires an app restart after first toggling it. If it still doesn't engage, check whether another app holds the device exclusively (some DAWs and Spotify with "exclusive" extensions do). The app will retry up to ~600 ms internally; longer holds will fail.

**Empty folders left after deleting an album.**
Hit **Refresh** in the Library tab — you'll be prompted to delete *truly empty* folders only (folders that still contain images or other files are left alone).

**Same-name artist downloads collide.**
The first download prompts you for a folder override; subsequent downloads from the same canonical Tidal artist ID re-use it silently. If you ever delete the artist folder entirely, the next download will prompt again so you can re-confirm.

**A track downloads as `.flac.partial`.**
The download was interrupted. The next attempt will replace the partial atomically. If a partial persists across sessions, delete it manually.

**The login modal won't go away.**
Make sure your network can reach `auth.tidal.com` and `api.tidal.com`. The app polls every 2 seconds during the device-code flow; check **Settings → Reset → Clear all favorites** is *not* what you want — use the Logout button on the Settings → Tidal section instead.

If something else breaks, please open an issue on the [Issues page](../../../issues) with the version, OS, steps to reproduce, and console output if available.
