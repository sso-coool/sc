# Releasing S;C

Use [GitHub Releases](https://github.com/sso-coool/sc/releases) to distribute Windows updates. The bootstrapper checks stable releases, so pushing a commit or saving a draft does not trigger an update.

1. Update the version in About and in the Windows file and product version resource. Use a four-part numeric tag such as `v0.3.0.8`.
2. Build the Windows x64 app from the reviewed source with the locked dependencies. Follow the build and path-remapping instructions in the latest release's `SC-source.zip`.
3. Check startup, version, DLL dependencies, and affected features. Review the executable, source archive, and metadata for private paths, credentials, logs, and other personal data. Keep the corresponding source and license notices with the build.
4. Create a draft release with the tag. Attach the files below, then verify their hashes and sizes before publishing it as stable. Use a new tag for corrected files.

| File | Purpose |
| --- | --- |
| `SC-windows-x64.zip` | Installable Windows build with `SC.exe`, `BUILD.json`, and license notices |
| `SC-source.zip` | Exact source for this Windows build, with build and installer scripts |
| `SC-bootstrapper-windows.zip` | Windows installer, launcher, and optional update scheduler |
| `SC-SHA256SUMS.txt` | SHA-256 checksums for the three archives |

The tag's automatic GitHub source download represents the repository contents. Download the separately attached `SC-source.zip` for the application source corresponding to the executable. `BUILD.json` records the executable and source hashes. The application is unsigned; release checksums verify that the files match those published.

After publishing, test the bootstrapper against the live release in a separate installation directory. Verify both the initial download and the no-update result at the current version. Include a short list of fixes and known limitations in the release notes.
