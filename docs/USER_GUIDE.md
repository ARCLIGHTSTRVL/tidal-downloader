# TIDAL DOWNLOADER — User Guide

**English** | [한국어](USER_GUIDE.ko.md)

This guide covers **v1.0.7**. See the [README](../README.md#download) for downloads, requirements and installation, or the [changelog](../CHANGELOG.md) for release history.

## Contents

- [First launch](#first-launch)
- [Search & discovery](#search--discovery)
- [Playlists](#playlists)
- [Audio quality](#audio-quality)
- [Album art](#album-art)
- [Downloading](#downloading)
- [Downloads page](#downloads-page)
- [Library](#library)
- [Tag editor](#tag-editor)
- [Player bar](#player-bar)
- [Audio device picker](#audio-device-picker)
- [Settings reference](#settings-reference)
- [Download naming](#download-naming)
- [Album-type grouping](#album-type-grouping)
- [Library maintenance](#library-maintenance)
- [Settings recovery and updates](#settings-recovery-and-updates)
- [Mouse & keyboard shortcuts](#mouse--keyboard-shortcuts)
- [Troubleshooting](#troubleshooting)

## First launch

1. Open the app and click **Login** in the sign-in dialog.
2. Complete sign-in in the Tidal window. Search, streaming and downloads require a working Tidal session and access to the requested music.
3. Open **Settings → Download location** and choose where to save music. To use an existing library, select its current folder.
4. Choose **Audio quality**, then search for an artist, album, track or playlist.

When you choose a download folder and no separate art folder is set, the app sets **Album art download location** to an `art` subfolder. You can change it separately.

Keep **Auto-refresh** enabled to let the app renew its session. Tidal may still require you to sign in again. **Settings → Language** switches between English and Korean; some controls retain their English labels.

## Search & discovery

Enter a name in the search bar. Select a result from the dropdown, or press **Enter** for the full results page. Results can include artists, albums, tracks and playlists.

The home view shows your favorited artists, **My Playlists**, and recent artists, albums, EPs, singles and playlists when those lists contain items. Use **View all** to open a longer list.

- Open an **artist** to browse albums and EPs/singles. The heart adds or removes the artist from the app's favorites.
- Open an **album** to see its tracks, play music or download it. Artist names link to their artist pages.
- Click a **cover** to open the larger image viewer, where available.

## Playlists

You can find playlists in search results or **My Playlists**, which combines playlists you created with playlists you favorited on Tidal. **Recent Playlists** contains playlists opened during the current session.

To open a playlist directly, paste a Tidal link containing `/playlist/<UUID>`, or just the playlist UUID, into the search bar and press **Enter**. The playlist must be accessible to your signed-in account. If a playlist is missing from My Playlists, try its direct link.

On a playlist page, click a track to play it, use its download icon for one track, or use the download icon below the playlist information for the full list.

Playlist downloads use their own layout:

```text
Download folder/
  playlists/
    Playlist name/
      01 - Track title.flac
      02 - Another track.m4a
```

Track numbers follow playlist order. The general folder and filename rules do not change this layout. The Library displays these files in a separate **Playlists** group, and the Tag Editor groups them by playlist folder. Downloaded album copies and playlist copies can coexist; playlist checkmarks refer to that playlist's copies.

The **✕ beside playlist progress** stops the batch before it starts the next track. The current track may finish. To request cancellation of that track as well, use its active download row or the Downloads page.

## Audio quality

**Settings → Audio quality** sets the requested tier for both playback and downloads.

| Setting | Requested audio | Fallback when needed |
| --- | --- | --- |
| **Max** | FLAC up to 24-bit / 192 kHz | Lower-resolution lossless, then High AAC |
| **HiFi** | FLAC at 16-bit / 44.1 kHz | High AAC |
| **High** | AAC at 320 kbps | AAC is expected at this tier |

The result depends on the track and what Tidal provides to your session. Choosing Max does not make every track high-resolution. Max and HiFi can both fall back to lossy **High**, saved as `.m4a`. FLAC downloads use `.flac`; the supplied audio is saved without re-encoding.

Check the actual track's quality information instead of relying only on the selected setting. **Check available quality** tests a small sample of tracks for your current session and reports lossless results or AAC downgrades. It does not establish availability for the whole catalog. Playback startup time varies with the track, connection and processing needed.

## Album art

**Settings → Album art quality** controls the requested resolution for art embedded in downloads:

| Setting | Requested size |
| --- | --- |
| **High** | 1280 × 1280 |
| **Standard** | 640 × 640 |
| **Low** | 320 × 320 |

The app tries a smaller size when the requested image is unavailable. A cover's enlarged viewer can also save art separately. **Album art download location** controls that destination; when unset, it uses the audio download folder.

Turn off **Album art hover tilt** in Settings if you prefer static cards.

## Downloading

- **One track:** click its download icon in Search, an album or a playlist. The player bar also has a download control.
- **An album:** use the download icon below its information. Tracks already marked as downloaded are skipped.
- **Cancel an album:** use the stop control beside its progress. This stops the batch and requests cancellation of its active tracks.
- **Cancel one attempt:** use **✕** on its active row in Downloads. Check the resulting status before retrying.

If an artist or playlist would share a folder name with a different one, the app may ask you for a distinct folder name. It reuses a recorded choice for later downloads of that artist or playlist.

A cancellation request does not mean that nothing reached disk. A file may already have been saved, or cleanup may still need attention. Read the result in Downloads before removing leftover files or starting another attempt.

## Downloads page

**Downloads** shows the current session's attempts, including progress and completed, failed or cancelled results. An attempt can report that audio was saved while tags, library registration or cleanup did not finish. Read those details even if the music file exists.

- Click a row to play the track **from Tidal**. To play the saved copy, open it in Library.
- **✕ on an active row** requests cancellation. On a finished row, it removes only that history entry.
- **Clear finished downloads** removes finished entries and leaves active attempts in the list.

This activity list clears when the app closes. Removing history entries does not delete audio or clear the library's downloaded checkmarks.

## Library

Library scans the configured download folder and presents local music by artist and album, with playlists in a separate group. Click a local track to play its saved file.

- **List:** expand artist, album and track rows. **Sort albums** offers Artist, Album, Year and Recently added. The list's selection is retained when you switch views.
- **Grid:** browse album and playlist covers, then open a card for its track list. The cover-size control changes card size.
- **Search:** filter your library by artist, album or track name.

**Refresh library** checks for legacy downloads that need conversion to standard FLAC, then rescans and refreshes artwork. In v1.0.5, this work is part of Refresh; there is no separate Remux button. A manual refresh can offer to remove empty folders or repair misplaced playlist files. Review the proposed action before confirming.

### Deleting local music

Delete controls in Library remove files **from disk**, not just from the view. Artist, album and playlist group deletions ask for confirmation; individual track controls act directly.

Group deletion targets the selected tracks. Artist and playlist cleanup may also remove associated `folder.jpg` art and empty directories. Other files, conflicts or files the app cannot safely verify can keep a folder from disappearing. If deletion is incomplete, the app refreshes the library and reports the remaining files. Close anything using them and review the result before retrying.

## Tag editor

Tag Editor loads the configured download folder and lets you add more files or folders with **Open Files**, **Open Folder**, or drag-and-drop. **Refresh** rereads loaded folders. **Clear directories** removes selected roots from the editor; it does not delete their disk files.

Open an album or playlist card to edit its files:

1. Select a track, change its metadata, then click **Save**. **Save Album** saves the edited tracks in the open group. **Revert** discards the selected track's unsaved metadata draft.
2. To change shared fields, use the album-level Album, Album Artist, Artist, Year, Genre or Composer fields and **Apply to All N Tracks**.
3. **Change Art** opens an image file picker. At album level, select the image and use **Apply to All N Tracks**. At track level, selecting an image writes it to that track separately from its metadata draft.

Saving tags can also move a file to match your saved folder and naming rules. Read the result: tag writing, file movement and library registration can have different outcomes. If the library is busy, tags may save while the move is deferred.

If a save is refused, interrupted or cannot be matched to the file currently shown, the editor can keep your draft and mark the row for attention. A retained draft does **not** prove the disk file is unchanged. Check the reported path and refresh before retrying, especially if another app moved or replaced the file. Drafts are working edits; save them before quitting.

For a deliberate folder cleanup, save pending edits, then use **Reorganize files to match folder structure**. Review moves, conflicts and any duplicate removals in the preview before applying. Reorganizing can remove a verified duplicate source when a matching destination copy is kept.

Legacy DASH-wrapped files may show that edits are stored only in the library index. Use **Library → Refresh library** to attempt conversion to standard FLAC, then reopen the file before continuing edits.

## Player bar

The player bar appears when a track is selected. It provides previous, play/pause, next, seek, volume and download controls.

- **Shuffle:** randomizes playback within the current album or folder.
- **Repeat:** cycles through off, one track, and the current album/folder.
- **Seek:** click or drag the progress bar to move within a track.
- **Quality badge:** shows the available quality information for the current track.
- **Cover and artist links:** open the corresponding detail view when available.

## Audio device picker

Open the speaker icon in the player bar, select an output, then open the selected device's gear icon for **Device settings**. Settings are stored per device.

- **Use exclusive mode:** requests the native output path, using WASAPI exclusive mode on Windows or Core Audio Hog Mode on macOS. The native audio component, device and driver must support the requested output. Other apps may lose access while the device is held.
- **Force volume:** available with exclusive mode. It locks the app's volume at 100% and disables its slider; use your device's volume control.

If native playback cannot start, the app can fall back to shared playback. Enabling the switch alone therefore does not verify exclusive or bit-perfect output. If the device is busy or silent, close other audio apps, select the output again, or turn exclusive mode off. Supported sample rates depend on the device and driver.

## Settings reference

Most settings save when changed. Download naming and album grouping have their own explicit **Save** button.

| Setting | What it does |
| --- | --- |
| **Account / Auto-refresh** | Sign in or out, and allow session renewal. |
| **Background playback & tray** | Closing the main window keeps the app running in the tray or menu bar. Use its menu to show the app or quit. |
| **Audio quality** | Requested tier for playback and downloads; see [Audio quality](#audio-quality). |
| **Album art quality / hover tilt** | Embedded artwork resolution and card animation. |
| **Download location** | Root folder for audio and the library being displayed. Choosing another folder does not move the old library. |
| **Album art download location** | Destination for separately saved cover art. |
| **Language** | English or Korean. |

### Reset

Both reset actions require a second click to confirm:

- **Reset library data** clears the app's favorited artists and cached images. It keeps downloaded files and downloaded checkmarks.
- **Reset current settings** restores playback, quality, download paths, artwork options and naming rules to defaults, and discards unsaved naming changes. It keeps language, Auto-refresh, the Tidal sign-in, downloaded files and the library index. If your library disappears afterward, select its existing download folder again.

## Download naming

Under **Settings → Download**, edit **Folder structure** and **File name**. These rules apply to future album/track downloads; playlists use the layout described [above](#playlists).

New settings use:

```text
Folder structure: {album_artist}/{album}
File name:        {track_number} - {title}
Example:          Adele/30/01 - Easy On Me.flac
```

Existing valid saved rules are kept when you update. Leave the extension out of the rule: the app adds `.flac` or `.m4a` as appropriate.

1. Use **Edit directly** to type a pattern, or **Build with tags** to construct it. Switching views preserves its text, tags and spacing.
2. Insert tags at the cursor or open **Tag help**. Separate folder levels with `/` or `\`.
3. Check **Path preview**. **Sample track information** lets you try missing metadata or multiple artists without changing real tracks or your rules.
4. Click **Save**. Leaving with unsaved naming changes offers Save, Don't save or Keep editing. **Cancel changes** restores the saved rules.

| Tag | Meaning / example |
| --- | --- |
| `{album_artist}` | Shared artist for the album: `Adele` |
| `{artist}` | Artists credited on the track: `Adele, Jessie J` |
| `{album}` | Album title: `30` |
| `{title}` | Track title: `Easy On Me` |
| `{track_number}` | Two-digit track order: `01` |
| `{year}` | Release year: `2021` |
| `{genre}` | Genre, when supplied: `Pop` |
| `{album_type}` | `Albums`, `EPs`, `Singles` or `Compilations` |
| `{bit_depth}` | Audio bit depth: `24` |
| `{sample_rate}` | Sample rate in kHz: `44.1` |
| `{quality}` | Downloaded audio quality: `Max` |

Use **Album artist** for one shared album folder. **Track artist** can put collaboration tracks in a different folder. Missing metadata can produce an empty value or a fallback name; check the preview's warnings.

**Presets** offers Default, Include year and Include quality. Include year uses `{album_artist}/{year} - {album}` for folders. Include quality adds `[{quality}]` to the filename, such as `01 - Easy On Me [Max].flac`.

Unknown tags remain literal text and show a warning. Unsupported path characters, reserved names and trailing dots or spaces are adjusted in the output. A filename extension typed into the rule becomes part of its name, so do not add one yourself.

## Album-type grouping

Open **Album grouping settings** in the naming editor and enable **Group albums by type**. With the default layout, downloads go into `Album artist/Albums/Album`, `EPs`, `Singles` or `Compilations`. **Merge compilations into Albums** uses the Albums folder for compilations too.

Click **Save** to apply this to future downloads. Existing files stay where they are until you explicitly reorganize them.

To organize an existing library:

1. Save your naming and grouping changes.
2. Open **Group existing library by album type → Preview**.
3. Review the current and proposed paths, skipped files and conflicts across the preview's pages.
4. Select **Apply**, then read the results for unfinished moves or library update warnings.

This preview uses your saved folder and filename rules, so inspect both parts of the destination. It is separate from the sample-track preview. Playlist files retain their locations. Unknown album types or custom templates with no clear album position can be skipped.

## Library maintenance

Use the operation that matches what needs fixing:

- **Resync from TIDAL (online)** fetches current metadata and previews tag, artwork, folder and filename changes using your saved rules. It requires internet. Review the preview before applying.
- **Rebuild from FLAC (offline)** rebuilds the download index from embedded Tidal identity in **FLAC and M4A** files, despite the button's name. It does not fetch fresh metadata or reorganize the audio. Files without usable identity information may be skipped.
- **Tag Editor → Reorganize** uses loaded files' metadata to preview folder and filename changes. Its preview can include duplicate removals, as described in [Tag editor](#tag-editor).

Keep the `TIDAL_GUID` and `TIDAL_META` fields when using another tag editor. They help the app recognize files and rebuild the index offline.

File reorganization changes disk paths, so read the proposed destinations and the final result. If the app reports a busy library, let downloads or other edits finish. If it refuses a symbolic link, junction or changed file, use the actual folder and refresh the file state before retrying.

## Settings recovery and updates

Settings can report that a backup was recovered, some values were repaired, or unsaved changes remain. If a save fails, keep the page open and use **Retry save**, or save the naming draft again. A recovery-backup warning can mean the main settings were saved but the backup was not updated.

If recovery or resetting selects the default download location, your previous audio files may simply be outside the displayed library. Choose their existing folder in **Download location**.

The app checks for new versions in the background. **Settings → Check for updates** also checks when the release service is reachable. A new version appears in a notification at the bottom right. From v1.0.7 onward, select **Official releases** in that notification to open the project's [GitHub Releases](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases) in your browser. If you are using v1.0.6 or earlier, open GitHub Releases directly or use the README download links.

Download the file for your OS, then follow the [manual update instructions](../README.md#updating-from-an-earlier-version). Quit the app, including its tray or menu-bar instance, before installing. Quitting does not automatically install an update, and existing valid saved settings remain in place. **GitHub Star** opens the project repository.

## Mouse & keyboard shortcuts

- **Mouse thumb buttons:** back and forward through available page history.
- **Click the current sidebar tab again:** return Search to home, or Library/Tag Editor to its root view.
- **Enter in Search:** open full results, or open a pasted playlist link/UUID.
- **Spacebar:** play/pause when a track is selected and focus is outside text fields and interactive controls.
- **Esc or a background click:** dismiss supported menus and dialogs. Operations that are applying changes may keep their dialog open.

## Troubleshooting

**Max or HiFi downloads are M4A.**
Tidal may have supplied AAC. Use **Check available quality**, inspect the actual track quality and see [Audio quality](#audio-quality). Renaming an M4A file to FLAC does not make it lossless.

**A download failed or was cancelled, but a file remains.**
Read the attempt's details in Downloads. Audio may have been saved before tags or cleanup failed. Let active work finish and verify the reported files before retrying or removing leftovers.

**The library looks empty after an update or reset.**
Check **Download location** first and select the folder that already contains your music. Refresh the library. If checkmarks or records are still missing, use the offline rebuild and review skipped files.

**Album art or artist photos look stale.**
Try **Refresh library**. **Reset library data** also clears cached images, but it clears favorited artists too; use it only when you want both reset.

**A folder remains after deleting music.**
It may contain images or other files, or deletion may have been refused. Read the result. Manual Refresh can offer cleanup of truly empty folders.

**Tag Editor still shows an unsaved or detached draft after saving.**
Check the save notice and disk file before retrying. Some writes may already have happened. Refresh after resolving external moves or replacements; use Revert only when you intend to discard that draft.

**Drag-and-drop does not work on Windows.**
Explorer cannot drop into an app running as administrator. Relaunch the app normally, or use Open Files / Open Folder.

**Exclusive mode fails, or the volume slider is locked.**
Try shared playback and close other apps using the device. Force volume intentionally locks the slider; disable it or control volume on your output device.

**The sign-in dialog keeps returning.**
Complete sign-in in the Tidal window and check network access. If Tidal rejects the session, follow its message and sign in again when available. Resetting favorites or naming rules does not fix authentication.

For other problems, open an [issue](https://github.com/ARCLIGHTSTRVL/tidal-downloader/issues) with the app version, OS, reproduction steps and relevant error messages. Remove account details, tokens and other private information from logs or screenshots before sharing.
