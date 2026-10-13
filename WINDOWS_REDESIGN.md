# VLC 3.0.24 — Windows Figma redesign

This branch starts from the **upstream VLC 3.0.24 tag** (commit `6de05adcbaf2e8b85fe86aad4169393098628119`) and targets **Windows only**.

Design source: [VLC Redesign — Community Figma](https://www.figma.com/design/9YWhUXvUqfd99fK7OGRNvg/VLC-Redesign--Community-)

## Current implementation

- Windows Qt Widgets palette and controls: charcoal surfaces, muted text, orange accent, rounded buttons, dark menus and media lists, dark settings inputs, tabs, and seek/volume sliders.
- Playlist sidebar: broader default space, adjustable from 240–427 px, while retaining the VLC 3.0 playlist model and folder/source selector.
- Existing libVLC playback, input management, subtitle operations and media decoding remain unchanged.
- All UI changes are conditional on `Q_OS_WIN`. macOS and Linux retain VLC's original UI.

These changes are a **visual foundation**, not a pixel-perfect implementation of all Figma frames. In particular, the redesigned title bar, player chrome, icon set, onboarding, settings layouts, and music library views are not yet implemented.

## Windows build

GitHub Actions workflow: `.github/workflows/windows-figma.yml`

The workflow cross-compiles on Ubuntu using Mingw-w64, invokes the source tree's `extras/package/win32/build.sh`, and attempts to upload unsigned Windows packages. Build logs are kept even if compilation fails. Because VLC 3.0.24 has a substantial third-party dependency graph, first builds may fail during tool/contrib setup and need iteration. A successful workflow should be checked before treating artifacts as usable Windows binaries.

The workflow runs on pushes to `windows-figma-vlc-3.0.24`, and can also be dispatched manually for x86_64 or i686.

## Next work items

1. Replace player control layout with Figma-inspired controls and responsive title/seek/volume regions while preserving shortcut and playlist wiring.
2. Extend the library panel's source navigation, file list and search UI.
3. Implement settings page groupings with native VLC configuration controls.
4. Match supplied icon and illustration assets rather than using placeholder graphics.
5. Verify playback, codecs, audio, video, subtitles, preferences, DPI scaling, light/dark behavior and Windows installer packaging.

## Licensing

VLC is GPL-licensed. Keep upstream copyright and license notices in modified source files. Ensure that any imported Figma graphics and assets have redistribution rights before including them in packaged binaries.
