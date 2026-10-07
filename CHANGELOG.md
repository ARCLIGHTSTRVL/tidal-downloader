# Changelog

[English](CHANGELOG.md) · [한국어](CHANGELOG.ko.md) | [User Guide](docs/USER_GUIDE.md) · [사용자 가이드](docs/USER_GUIDE.ko.md)

All notable user-visible changes to TIDAL DOWNLOADER.

## v1.0.7 - Manual updates from GitHub Releases

- Keep version checks and the new-version notification at the bottom right. Its **Official releases** button opens the project's GitHub Releases page in your browser for manual download and installation.
- Disable automatic update installation when quitting the app.
- Retain existing valid saved settings and naming rules.

See the [v1.0.7 release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.7) for downloads and update guidance.

## v1.0.6 - Windows playback and Tag Editor fixes

- Fix a blank console that could appear when starting or seeking streamed or downloaded music on Windows.
- Make the Tag Editor's album sort tooltip and menu follow the English/Korean language setting without changing the selected sort order.

See the [v1.0.6 release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.6) for downloads and update guidance.

## v1.0.5 — 2026-10-04

### Improved

- Edit folder and filename rules directly or with tags without losing the complete rule when switching views.
- Use presets, tag help and sample-track previews, then explicitly save naming changes. Existing valid saved settings are retained; new settings default to `Album artist/Album` folders and `Track number - Title` filenames.
- Optionally group releases into `Albums`, `EPs`, `Singles` and `Compilations`, with a choice to merge compilations into `Albums`. Preview and explicitly apply changes to existing library files.
- Choose album title, year or recently added in the Library's list-view sort menu; keep the selected order when switching views.
- Improve result reporting for tag edits, file moves and deletion, and download completion or cancellation.
- Improve track changes and cancellation during playback preparation, and cleanup of active work when quitting.
- Show settings recovery and save-failure feedback. Add a GitHub Star link below the update check.

### Installation notes

- Windows x64 and macOS arm64/x64 downloads are available. macOS requires Monterey 12 or later.
- Use a manual update with the current signing setup. The Windows installer is self-signed; Mac builds have no Apple Developer ID signature or notarization.
- The Intel Mac build was exercised under Rosetta on Apple Silicon; Intel hardware was not tested.

See the [v1.0.5 release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.5) for downloads and update guidance.

## v1.0.4 — 2026-09-27

- Prevent hard-to-access Windows folders whose names end in a period or space.
- Improve album browsing when deluxe and other editions were missing.
- Clarify Artist and Album Artist folder choices and improve folder/file templates.
- Improve recently-added Library sorting.
- Fix AAC/M4A identity metadata and restoration of download-complete indicators.
- Match library-record recovery warnings to the file's recovery-information state.
- Label High AAC as lossy by design and distinguish it from a downgraded lossless request.
- Improve update notification behavior and Korean/English guidance.

See the [v1.0.4 release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.4) for the original update instructions and limitations.

## v1.0.3 — 2026-08-14

### New

- **macOS support** — native Apple Silicon and Intel builds with **Core Audio Hog Mode** exclusive output and device sample-rate switching, a macOS-native titlebar and menu-bar tray, and library path support. Supersedes `v1.0.2-beta`; the Intel build now ships the correct Intel ffmpeg, fixing downloads and conversion in that build.
- **Playlist browsing** — find Tidal playlists in search, your own and favorite playlists, or recently opened playlists. Paste a playlist link or UUID into the search bar to open it directly.
- **Playlist downloads** — save a whole playlist to `playlists/<name>/` with playlist-order filenames and a `folder.jpg` cover. A folder-name prompt separates playlists that share a name.
- **Playlists in the Library** — a dedicated group in list and grid views, with per-playlist and per-track deletion. Detect misplaced album folders inside `playlists/` and move them back; identify duplicates using embedded IDs and file-byte comparisons.
- **Max, HiFi and High downloads** — Max (24-bit FLAC up to 192 kHz), HiFi (16-bit / 44.1 kHz FLAC), and now High (AAC 320 kbps) saved as `.m4a` with embedded identity, checkmarks, and offline Rebuild support.
- **HiFi login fix** — changed the login client to address HiFi requests returning AAC instead of the requested 16-bit / 44.1 kHz FLAC.
- **Update notifications** (Windows + macOS) — background version checks with a toast linking to the newest release, plus a manual check in Settings.
- **English / 한국어** — switch the UI language instantly in Settings.
- **Parallel album downloads** — album tracks download 3 at a time on both platforms.
- Search home shows library stats; library search matches playlists and their tracks.
- **Space bar** toggles play/pause while the app is focused (never while typing).

### Improved / Fixed

- Search navigation is symmetric — back from an album/artist restores your results; clicking a track no longer wipes them.
- Downloads panel ✕ cancels an in-progress download; Clear removes only completed or failed entries. Cancellation is also checked after transfer and validation steps, before publishing the file.
- Fixed a duplicate space-bar handler that toggled play/pause twice per press. Holding the key no longer repeats the toggle.
- An expired Tidal session shows an error banner instead of pretending there are no results.
- Update-check failures show concise English or Korean guidance for connection problems, missing update information or other errors, replacing raw HTTP responses, stack traces and local paths in notifications and Settings.
- Player overlay: correct per-track artists, ALBUM INFO and the queue follow the current song's album or playlist, and album changes no longer retain the previous cover. Closing the overlay also has a corrected rotation animation.
- Library toolbar stays pinned while scrolling, with consistent spacing in the playlist grid. Unreadable folders show a warning banner instead of appearing to be an empty library.
- Failed deletes show a summary and keep failed items visible in the Library and search results.
- Library grouping uses artist IDs, album IDs and folders, and playlist UUIDs to separate same-name entries.
- Added embedded-identity checks for downloaded-✓ marks, helping recognize renamed or retagged files. Some legacy indexed files can still match by title when embedded identity is not required.
- Settings polish: consistent controls, Reset no longer wipes language or auto-refresh preferences, hold-to-delete requires a real 1-second hold.

### Reliability

- Direct downloads follow redirects and reject HTTP errors or transfers shorter than the declared size. File-signature checks, metadata parsing and an ffmpeg first-frame decode check run before a download is accepted.
- Downloads use a separate temporary file for each attempt. File ownership and collision checks choose a suffixed filename when an existing destination cannot be replaced as the same track.
- DASH remux runs before the download is published; a remux failure reports a failed download. In-place FLAC remux no longer deletes the original before attempting replacement.
- Downloads and library maintenance are coordinated so file moves, reorganizing and Rebuild do not run alongside active downloads.
- Deletion checks the selected files and library boundaries, refuses symlink/junction traversal, and leaves unverified content in place. Only deleted files are removed from the index.
- Library index saves write and validate a temporary copy before replacing the previous index; save failures are reported.
- Offline Rebuild reads identity tags from FLAC and M4A files. It retains existing records for files it cannot rebuild unless the file is confirmed missing.

See the [v1.0.3 release notes](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.3) for the original downloads and installation notes.

## v1.0.2 — 2026-05-01

### Branding
- App name standardized to **TIDAL DOWNLOADER** (uppercase) across the installer, Start menu / shortcut, system tray, splash screen, and DMG title — matching the in-app titlebar and Settings footer that already used the uppercase styling. Existing user data (settings, favorites, Tidal token) is preserved automatically. On Windows, you may want to remove the old "Tidal Downloader" entry from Add/Remove Programs before installing the new build, since NSIS treats different product names as separate entries.

### New
- **Bit-perfect WASAPI exclusive output** (Windows). The app can now grab the audio device exclusively and output at the source's native sample rate / bit depth (16/44.1, 24/96, 24/192) without the OS audio engine in the path.
- **Force-volume toggle** in the audio device picker. When exclusive mode is on, you can lock playback to 100 % volume so the slider stays out of the bit-perfect signal.
- **Album art quality selector** (Settings → Album art quality). Choose 1280, 640, or 320 — embedded into downloads and used in the lightbox, with a graceful fallback chain if a size isn't available for a given album.
- **Album art download location** — separate the audio path from the art path; defaults to `<downloadPath>/art`.
- **Hover tilt** on album cards and large covers across Library / Search / Tag Editor (toggle in Settings).
- **Image lightbox** for album covers and artist profiles, with click-to-download.
- **Per-artist delete** button in the Library grid view (with confirmation) — removes the folder + scrubs the library index.
- **Empty-folder cleanup prompt** on Library refresh.
- **Reset section** in Settings — reset download history, clear favorites, clear image cache.

### Improved
- **Faster Max-quality (DASH) downloads** — segments now download in parallel (concurrency 4) with HTTP keep-alive, atomic `.partial → rename`, per-segment retry × 3, and protection against double-click duplicate downloads.
- **Same-name artist handling** (e.g. *LiSA* vs *LISA*) — the app now prompts for a folder override the first time and remembers your choice via canonical Tidal artist IDs.
- **Track variant titles** (Extended / Instrumental / Sped Up / Remastered) are now merged into filenames and tags via Tidal's `version` field.
- **Auto-advance playback** falls through from album → library-wide flat list when an album finishes.
- **Repeat / shuffle controls** in the player bar — three-state repeat (off → one → album) with independent shuffle (off / album).
- **Pointer-event seek scrubber** in the player bar (Spotify/YouTube-style, no flicker on drag).
- **Player bar layout** — center column expands responsively up to one-third of the window; shuffle/repeat keep a fixed 72 px distance from prev/next.
- **App-wide back/forward** with mouse thumb buttons — works across Search, Library, and Tag Editor pages.
- **Sidebar re-tap** of the current page resets that page (Search → home, Library → list root, Tag Editor → grid root).
- **Login modal** appears on launch when the Tidal token is missing or expired (in-place, sidebar still navigable).
- **App icon** has rounded corners.
- **Splash screen** is shown for at least 1 s (was 600 ms).

### Fixed
- **Windows taskbar icon** now displays correctly (icon was missing from the build whitelist + AppUserModelId was not set).
- **Album art "High" quality** is now actually 1280 × 1280 — previously the CDN's 4xx response on missing sizes was silently swallowed.
- **Search results no longer flash the home view** during the loading transition.
- **Stale artist profile pictures** when re-searching the same artist — now fetched canonically every time.
- **Tag Editor** album-detail layout no longer breaks at narrow widths (responsive grid).
- **Player album overlay** no longer flips backwards when the track changes.
- **Player bar seek bug** that could cause playback to freeze at 100 % is gone (scrubber rewrite).
- **Audio device exclusive grab** is now reliable when other apps are already playing — three-tier retry from JS down to the C++ render thread.
- **Force volume** stays at 100 % across track changes.
- **Album-art Lightbox download** now validates the actual image bytes before writing.
- **Installer** no longer shows the "already installed" prompt twice on UAC re-elevation.
- **ffmpeg path** in the packaged build correctly resolves into `app.asar.unpacked` (would previously ENOENT on first playback / DASH remux in the installed app).

### Removed
- Dropped the `audify` dependency (replaced by the in-tree WASAPI native module).

---

## v1.0.1 — 2026-04-26

Initial public release on Windows.

### Highlights
- Tidal OAuth Device Code login with auto refresh
- LOSSLESS playback (HiFi 16-bit / 44.1 kHz FLAC) with selectable download quality
- DASH / BTS manifest handling with ffmpeg remux for Max quality
- Library scanner (Artist > Album tree) and Tag Editor
- Search with discography view, favorites, recent history
- Custom dark UI with frameless window and macOS-style buttons
- NSIS installer with upgrade / downgrade / repair detection
- **Probe Tidal quality availability** (Settings → Check available quality) — diagnoses Tidal's AAC silent-downgrade by sampling two short test tracks per tier and reporting whether each one came back as FLAC or AAC.
- **`TIDAL_GUID` / `TIDAL_META` Vorbis comments** embedded into every downloaded FLAC at write time, enabling offline rebuild of the library index from the audio files themselves.
- **Library maintenance** — *Resync from TIDAL (online)* re-fetches metadata + re-applies your folder/naming rules to existing files; *Rebuild from FLAC (offline)* reconstructs the library index from the embedded Vorbis comments.
- Splash window (1280 × 1280, ≥ 600 ms minimum), single-instance lock, spacebar play/pause, album catalog pagination (limit = 50), DevTools blocked in packaged builds.

(See [Releases](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases) for the original release notes and downloads.)
