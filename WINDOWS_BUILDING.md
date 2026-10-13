# Windows builds for the VLC 3.0.24 redesign

The feature branch starts at the upstream VLC 3.0.24 commit
`6de05adcbaf2e8b85fe86aad4169393098628119`.
It builds only Windows packages (cross-compiled on a Linux CI host).

## Build environment

Use VideoLAN's **VLC 3.0-specific** CI containers, not an arbitrary new
distribution toolchain. The exact images are sourced from this tag's
`extras/ci/gitlab-ci.yml`:

- Windows x64: `registry.videolan.org/vlc-debian-win64-3.0:20260820114402`
- Windows x86: `registry.videolan.org/vlc-debian-win32-3.0:20260820114402`

The official VLC 3.0 CI includes special MinGW GCC compatibility choices.
Qt 5.6.3's MOC parser fails on `iterator_concepts.h` in Ubuntu 22.04's
newer toolchain. In addition, mixing host and target `CC/CXX` prevents
Linux build tools from executing. The CI image avoids these incompatibilities.

## Contribs

The build optionally checks for the exact official contrib archive
corresponding to VLC 3.0.24. Its upstream last-contrib commit is
`910cc9cada6080dd777fbafb2ebdd0c24f702c30`, using the path set from
`extras/ci/get-contrib-sha.sh` (and the Windows-specific build path).
The URL must respond successfully before the workflow enables `-p`.
Otherwise, the workflow rebuilds contribs from source. Compatible prebuilt
contribs MUST use the same C++ toolchain and runtime.

## Compilation and artifacts

The official build script is `extras/package/win32/build.sh`.
The workflow runs `-r -a x86_64 -i u` by default. Manual dispatch can
select `i686`. It sets `JOBS=2` and `MESON_BUILD="-j 2"`; a Windows
build.sh adjustment allows an externally specified JOBS value.

The complete Windows distribution is produced under `win64/` (or
`win32/` for i686), NOT the repository root. Successful runs upload
EXE, MSI, ZIP and/or 7z packages from these directories when present;
unsuccessful builds upload `windows-build.log`.

## References

- `extras/ci/gitlab-ci.yml` (same VLC 3.0.24 source)
- `extras/ci/get-contrib-sha.sh` (exact contrib version selection)
- `extras/package/win32/build.sh` (upstream-supported build process)
- https://wiki.videolan.org/Category:Building/
- https://github.com/videolan/vlc/blob/master/doc/BUILD-win32.md
  (newer branch; useful high-level guidance, not a replacement for 3.0 CI)

A passing package step is only a compilation/packaging verification.
Actual playback, video embedding and redesigned UI require Windows testing.
