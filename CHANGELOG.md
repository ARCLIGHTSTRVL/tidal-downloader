# Changelog

All notable user-visible changes to TIDAL DOWNLOADER.

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

(See repository for the full code-level history starting v1.0.2.)
