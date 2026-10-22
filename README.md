# NeonChat Releases

Official public distribution files for NeonChat. Application source remains private. The website reads `RELEASE_MANIFEST.json` on `main` at download time; Git LFS stores the installer bytes. An LFS pointer is not an installer.

| Platform | Version / build | Installer | Architecture |
| --- | --- | --- | --- |
| Android | 1.0.0 / 15 | NeonChat-1.0.0-build15-Android.apk | arm64, armv7, x86_64 |
| Windows | 1.0.0 / 1 | NeonChat-1.0.0-build1-Windows-x64-Setup.exe | x64 |
| macOS | 1.0.1 / 5 | NeonChat-1.0.1-build5-macOS.dmg | Apple silicon and Intel |

No signed distributable iOS package is available. iOS remains Coming Soon.

## Integrity and verification limits

`SHA256SUMS.txt` and the manifest record the actual installer SHA-256 hashes. With materialized files, run `shasum -a 256 -c SHA256SUMS.txt` on macOS/Linux, or `Get-FileHash <installer> -Algorithm SHA256` on Windows. Checksums prove byte agreement, not publisher trust.

Android is a signed release APK, with versionCode 15/versionName 1.0.0, three ABIs, valid ZIP CRC and AOT libraries. The bundled environment contains only public client settings. macOS disk-image integrity and deep strict code signature verify; its embedded app is universal 1.0.1 build 5 with the NeonChat icon. The DMG is ad-hoc signed, not notarized. Windows is the supplied PE/Inno Setup installer matching the successful packaging record and companion x64 app 1.0.0+1; it is unsigned. Windows runtime was not repeated on the publisher's Mac. Publishing these packages does not claim fresh messaging, calling or notification tests.

Older Android build14 is deliberately excluded because it contained a server credential. Never add it or any unreviewed original. Originals are preserved privately.

## Downloading

The official website resolves the current manifest and downloads the exact immutable GitHub media URL for its selected commit. For a local checkout, install Git LFS, clone this repository, then run `git lfs pull` before opening an installer. GitHub's raw endpoint returns LFS pointers; `media.githubusercontent.com/media/yashasraj615/Neon-Chat-Releases/<commit>/<filename>` returns bytes.

## Publishing a new version

1. Fetch current `origin/main`. Review the remote manifest before choosing a release. Do not use filenames or timestamps as the version authority.
2. Inspect embedded native version/build, architecture, release signing, installer completeness and runtime evidence. Inspect bundled configuration for secrets. Do not publish failed, debug, source, incomplete or unreviewed artifacts.
3. Add the new, uniquely versioned installer. Never replace the bytes at an existing version/build identity. Calculate SHA-256 and size from the complete actual file.
4. Keep only the highest verified stable version/build per platform in the manifest. Versions use three numeric components; compare numerically (1.10.0 > 1.9.0), then numeric build. Preserve other platforms. Before publication, run the official landing project's read-only `scripts/check-release.mjs` with the freshly fetched previous manifest, candidate manifest and installer directory. It rejects rollbacks, rewritten identities and missing/inconsistent files and checks actual size/hash/container markers. Native validation remains required separately.
5. Update `SHA256SUMS.txt` with the manifest. Stage only the verified files, checksums and manifest. Review the staged diff and secret scan. APK/EXE/DMG must remain LFS tracked.
6. Upload LFS objects first (`git lfs push origin main`), then push the commit containing pointer files, checksums and manifest together (`git push origin main`). This publishes one consistent Git snapshot. Never force push. If another publisher has advanced main, stop, fetch and repeat the comparison/preflight; do not bypass the non-fast-forward rejection.
7. Check anonymous manifest/pointer/media reads and download each complete public installer again to verify size and SHA-256. If the new release is unavailable, fix its publication; never roll the manifest back to silently serve an older package. Publish a higher build for corrections.

No application credentials, private source, signing material or build intermediates belong in this repository. Git LFS transfer availability and quotas remain subject to GitHub service limits.
