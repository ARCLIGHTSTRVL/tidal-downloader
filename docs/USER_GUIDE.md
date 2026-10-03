# TIDAL DOWNLOADER — User Guide

This guide describes v1.0.5. For downloads, supported operating systems, installation and manual updates, see the [README](../README.md#download).

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
- [Audio device picker](#audio-device-picker)
- [Settings reference](#settings-reference)
  - [Account](#account)
  - [Playback](#playback)
  - [Audio quality](#audio-quality-1)
  - [Album art quality](#album-art-quality-1)
  - [Download](#download)
  - [Album-type grouping](#album-type-grouping)
  - [Library maintenance](#library-maintenance)
  - [Settings recovery and updates](#settings-recovery-and-updates)
  - [Reset](#reset)
- [Mouse & keyboard shortcuts](#mouse--keyboard-shortcuts)
- [Troubleshooting](#troubleshooting)

---

## First launch

1. The app opens with a Tidal sign-in modal. Click **Login**.
2. Complete authentication in the Tidal sign-in window.
3. After successful sign-in, the app returns to its main view.
4. Open **Settings → Download location** and pick a folder. The album-art folder is initialized to `<downloadPath>/art` automatically; you can change it under **Album art download location**.

Keep **Auto-refresh** enabled in Settings to let the app renew its session in the background. Tidal may still require you to sign in again.

## Audio quality

Choose the quality to request in Settings:

| Setting | Requested quality | Possible fallback |
|---------|-------------------|-------------------|
| **Max** | FLAC up to 24-bit / 192 kHz | Lower-resolution FLAC, then High AAC |
| **HiFi** | FLAC at 16-bit / 44.1 kHz | High AAC |
| **High** | AAC at 320 kbps | — |

**Settings → Audio quality** controls both playback and download. Max takes ~1–3 s on first play because the app assembles DASH segments before sending to ffmpeg; HiFi plays instantly when Tidal serves it as 16-bit FLAC.

### Quality fallback when a track isn't available at the requested tier

The app tries the selected tier and then lower tiers when needed. Max and HiFi both allow a final High/AAC fallback:

- **Max requested, high-resolution FLAC available**: saves the available FLAC at its supplied bit depth and sample rate.
- **Max requested, only HiFi FLAC available**: saves the lower-resolution FLAC.
- **HiFi requested, lossless available**: saves FLAC.
- **Max or HiFi requested, only High AAC available**: the app can continue at High and save `.m4a`. This is a lossy download even though the original setting requested lossless audio.
- **High requested**: AAC is expected and is saved as `.m4a`.

FLAC is saved as `.flac` and AAC as `.m4a`, without re-encoding the audio. Check the quality shown for the actual track. **Settings → Check available quality** samples what your account can fetch; another track may return a different quality.

## Album art quality

**Settings → Album art quality** controls the resolution embedded into your downloaded files and shown in the lightbox.

- **High** — 1280 × 1280 (best, larger file)
- **Standard** — 640 × 640 (good balance)
- **Low** — 320 × 320 (small)

The app uses a fallback chain: if Tidal doesn't have the requested size for an album, it tries the next-smaller until one works. Embedded art is verified by JPEG/PNG/WebP magic bytes, so a stray HTML error response from the CDN never gets stamped into your tags.

## Search & discovery

The home view has four sections:

- **Favorites** — heart-marked artists
- **Recent Artists**
- **Recent Albums**
- **Recent EP & Singles**

Each section auto-fits two rows; click **View all** when there's more.

The top search bar is sticky. Hit Enter to switch to the full search-results grid.

Click any artist to open their **Artist page** with a 180 × 180 circular avatar, a heart for favoriting, and split sections for *Albums* and *EP & Singles*. Clicking an artist's name from any other view (player bar, album detail, library grid) navigates here using the canonical Tidal artist ID — so *LiSA* and *LISA* never collide.

Click any album to open its **Album page** with a large cover (which you can hover-tilt and click for the full-resolution lightbox), the track list, and per-track download buttons.

## Downloading

- **Single track** — click the download icon next to a track.
- **Whole album** — click the download icon below the album information. Tracks already downloaded (✓) are skipped automatically.
- **Stop** — a stop button appears next to the progress bar; cancels the album-level download or an individual track.
- **Progress** — the progress bar increases monotonically across an album, including resume scenarios. No flicker between tracks.

The first time you download from an artist whose folder name conflicts with an existing same-name artist, the app prompts you for an override (e.g. `LiSA (KR)`). The next download from that same canonical artist ID re-uses your override silently.

**Album art quality = High** requests 1280 × 1280 art, with a smaller image used when that size is unavailable.

## Library

The Library tab scans your configured download folder and groups indexed tracks by artist and album. New settings use `Album artist/Album` folders; saved custom layouts and optional album-type folders are also supported.

Two view modes:
- **List** — Artist → Album → Track tree. Currently-playing track is highlighted in cyan.
- **Grid** — artist sections with circular avatars (auto-fetched from Tidal) and album-art cards. Click an album to open a 75 %-width detail view with the large cover and track list.

In List view, use **Sort albums** to choose album title, year or recently added. The selected list order is retained when you switch to Grid and back.

In Grid mode, each artist section header has an `✕` button (top-right). Clicking it asks for confirmation, then deletes the entire artist folder recursively and scrubs the library index.

The **Refresh** button (top-right) re-scans, then prompts you to delete any *truly empty* folders (no audio, no images, nothing). The first scan on app launch never prompts — only manual refreshes do.

The **Remux** button performs a one-shot conversion of any DASH-wrapped FLAC files in your library into standard FLAC (with tags + album art embedded). Useful for legacy downloads from earlier app versions.

You can play any local file by clicking it — the app uses a custom `local://` protocol that preserves the native bit depth and embedded album art.

## Downloads page

The **Downloads** tab in the sidebar (between **Tag Editor** and **Settings**) shows download activity at a glance.

- **In progress** — currently-downloading tracks with their album art, title, and a real-time progress bar.
- **Completed / failed / cancelled** — a separate result for each attempt. If a file was saved but tags, the library index or cleanup could not finish, the row explains which part needs attention.
- **Click a row** — plays that track from Tidal (uses the streaming path, not the local file).
- **✕ on an active row** — requests cancellation. On a finished row, it removes the history entry without deleting the audio file.
- **Clear finished downloads** — clears completed, failed and cancelled entries while leaving active downloads in the list.

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

Check the result after saving. A message may distinguish completed tag writes from a deferred file move or an index update failure. If a file was moved or replaced outside the app while being edited, refresh it before trying again.

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

## Audio device picker

The speaker icon opens your system audio devices. Select an output, then click its gear icon for **Device settings**:

- **Use exclusive mode** — requests WASAPI exclusive output on Windows or Core Audio Hog Mode on macOS. Device support and access determine whether the mode can be used; other apps may be unable to use that output while it is held.
- **Force volume** — locks the app's playback volume to 100 %. Only available when exclusive mode is on.

If the device is busy or the mode cannot be opened, close other applications using it or try shared playback. Available formats depend on the output device.

## Settings reference

The Settings page groups options by purpose. Each section is described below.

### Account

- **Tidal** — Sign in through the Tidal sign-in window or sign out. The current account is shown when signed in.
- **Auto-refresh** — Lets the app renew its Tidal session in the background. Leave this enabled for normal use; an expired or rejected session may still require a new sign-in.

### Playback

- **Background playback & tray** — When on, closing the main window minimizes the app to the system tray instead of quitting. Right-click the tray icon to **Show TIDAL DOWNLOADER** or quit. Use this if you want the app to keep playing music while it's out of the way.

### Audio quality

This section sets the requested audio tier for **both playback and downloads**. The detailed timing characteristics are described in the [Audio quality](#audio-quality) section above.

- **Max** — 24-bit HiRes (DASH, remuxed to standard FLAC).
- **HiFi** — 16-bit / 44.1 kHz FLAC (BTS, single URL).
- **High** — AAC.

#### Check available quality

Runs a quick probe against Tidal to see which tiers actually return lossless audio for your account right now. The app fetches two short sample tracks (one from your library, one a global hit) at each tier and reports whether Tidal delivered FLAC or AAC.

High is **lossy by design**; AAC at this tier is expected and can be downloaded as `.m4a`. AAC returned for a Max or HiFi request is marked as a downgrade. The probe reports only its sampled tracks, so availability for another album may differ. See [quality fallback](#quality-fallback-when-a-track-isnt-available-at-the-requested-tier).

### Album art quality

Resolution of the album art embedded into downloaded files and shown in the lightbox. Falls back to the next smaller size automatically if Tidal doesn't have the requested resolution for an album.

- **High** — 1280 × 1280 (best, larger file)
- **Standard** — 640 × 640 (good balance)
- **Low** — 320 × 320 (small)

- **Album art hover tilt** — When on, album cards smoothly tilt toward the mouse cursor (3D parallax) across the Library, Search, and Tag Editor pages. Toggle off if you find it distracting; cards remain static.

### Download

- **Download location** — Root folder for downloaded audio. Setting this for the first time also initializes the album-art folder to `<downloadPath>/art`.
- **Album art download location** — Separate folder for full-resolution art saved from the lightbox. If unset, the lightbox download falls back to the audio folder.
- **Folder structure** — Defines folders below the download location. The new-settings default is `{album_artist}/{album}`.
- **File name** — Defines the filename without its extension. The new-settings default is `{track_number} - {title}`. The correct audio extension, such as `.flac` or `.m4a`, is added automatically.

Existing valid saved rules take precedence over these defaults.

#### Editing and saving naming rules

1. Choose **Edit directly** or **Build with tags** for either field. Both views show the same rule; switching views preserves its tags, text, punctuation and spacing.
2. Type a rule or insert tags from the controls for that field. **Tag help** lists syntax and examples; **Presets** offers Default, Include year and Include quality.
3. Check **Path preview**. **Sample track information** changes only the example data, not your rule or actual songs. The multiple-artists example differs when the rule uses **Track artist**.
4. Select **Save** to apply the rules to future downloads. Leaving with unsaved changes prompts you to save, discard or keep editing.

Unknown tags remain literal text and produce a warning. Characters that cannot be used in filesystem paths are adjusted in the output. Preview the resulting folder and filename before saving.

### Album-type grouping

Enable **Group albums by type** to add `Albums`, `EPs`, `Singles` or `Compilations` to the default folder layout, for example `Album artist/Albums/Album`. **Merge compilations into Albums** puts compilations in `Albums` too.

Saving applies this setting to future downloads. It does not move existing files automatically.

To group existing files by album type, save your naming changes, then open **Group existing library by album type → Preview**. Review current and proposed paths, including skipped files and conflicts, then select **Apply**. This preview uses the saved folder and filename rules; it is separate from the sample-track preview above. Playlist files keep their existing locations. Unknown album types or ambiguous custom folder templates may be skipped.

### Library maintenance

Actions for keeping your downloaded library tidy. None of them delete audio files.

- **Resync from TIDAL (online)** — Fetches current Tidal metadata and previews tag, folder and filename changes using saved rules. Review the preview before applying. Requires internet and may update embedded album art as well as tags.
- **Rebuild from FLAC (offline)** — Despite its current label, rebuild reads embedded `TIDAL_GUID` / `TIDAL_META` identity from both FLAC and M4A. It reconstructs the library index from accessible files without a network request. Use it after moving files or recovering an index; missing or damaged identity tags can prevent recovery of some entries.

If a file operation is refused because the library path crosses a symbolic link or junction, use the actual folder path. Review the operation's results for skipped files or unresolved changes.

### Settings recovery and updates

The Settings page reports recovered settings, unsaved changes and recovery-backup failures. If a save fails, keep the page open and use **Retry** or save the naming draft again. A backup warning can mean the main settings file was saved but its recovery copy was not updated.

**Check for updates** checks for a newer version. With the current signing setup, download the appropriate release file and update manually after quitting the app. The **GitHub Star** link below the update check opens the project repository.

### Reset

Clear cached state without touching your audio files.

- **Reset library data** — Clears your favorites list and the Chromium HTTP cache (which holds Tidal CDN images) in one step. Use this when favorites need a fresh start or when artist profile photos / album art appear stale. Two-step confirmation: the first click turns the button red and waits up to 5 seconds for a second click to actually run.
- **Reset current settings** — Restores playback, quality, download paths, album-art options and naming rules to their defaults, and discards unsaved naming changes. It keeps language and auto-refresh preferences, downloaded files, the library index and the Tidal sign-in. Same two-step confirmation.

## Mouse & keyboard shortcuts

- **Mouse thumb buttons (XButton1 / XButton2)** — app-wide back / forward, including across pages (Library album detail ↔ Library grid, Search album ↔ artist page, Tag Editor album detail ↔ grid).
- **Sidebar re-click on the current page** — resets that page (Search → home; Library → list/grid root; Tag Editor → grid root). Clicking a different page tab navigates without resetting state.
- **Search bar Enter** — switches to the full results grid (from the dropdown overlay).
- **Spacebar** — play / pause (when no input field is focused).
- **Esc / background click** — closes most modals (lightbox, Audio Device settings, etc.).

## Troubleshooting

**The album art doesn't update after re-download.**
Open **Settings → Reset → Reset library data** (two-step confirmation) to clear cached images. This also clears favorites, so use it only if you want both reset.

**Library grid shows the wrong artist photo.**
Hit **Refresh** in the Library tab — the avatar cache is cleared on every refresh, so the next pass fetches fresh data via canonical artist IDs.

**Search results take me to the home view briefly before the album loads.**
This was a v1.0.1 bug. Update to v1.0.2 or later.

**Exclusive mode does not engage / volume slider doesn't move (Windows).**
Check whether another app holds the device, and try shared playback if exclusive access fails. **Force volume** intentionally locks the slider at full volume while exclusive mode is enabled.

**Empty folders left after deleting an album.**
Hit **Refresh** in the Library tab — you'll be prompted to delete *truly empty* folders only (folders that still contain images or other files are left alone).

**Same-name artist downloads collide.**
The first download prompts you for a folder override; subsequent downloads from the same canonical Tidal artist ID re-use it silently. If you ever delete the artist folder entirely, the next download will prompt again so you can re-confirm.

**A track downloads as `.flac.partial`.**
Check the download's result in **Downloads** before retrying. An interrupted operation may report leftover files or cleanup still in progress; do not remove a file while a download is active.

**The login modal won't go away.**
Complete sign-in in the Tidal window and check that your network can reach Tidal. If Tidal rejects the sign-in, follow the provider's message. Clearing favorites or resetting naming rules will not resolve an authentication failure.

If something else breaks, please open an issue on the [Issues page](../../../issues) with the version, OS, steps to reproduce, and console output if available.
