# S;C

A native Discord client for Windows with a compact interface, editable themes, and a focus on modest memory use. Built with Rust, Slint, and Skia.

**Unofficial, experimental, and unsigned.**

## Download

Get the Windows x64 app, the bootstrapper, the matching source, and checksums from [the latest release](https://github.com/sso-coool/sc/releases/latest). S;C targets Windows 10 and 11 on x64.

- For a portable copy, extract `SC-windows-x64.zip` and run `SC.exe`.
- For guided installation, run `SC-Setup.exe`. For command-line setup, extract all of `SC-bootstrapper-windows.zip` and run `Install-SC.cmd -Launch`.
- To update S;C, open **Settings → About** and select **Check now**. `Start-SC.cmd` also checks for a stable release when it starts the app.

## Automatic updates

Run `Enable-SCUpdates.ps1` to schedule a check every six hours. Updates wait until S;C is closed by default. Add `-RestartWhenRunning` to allow automatic restart, which can interrupt calls. Use `-EveryHours 1` to check hourly or `-Disable` to stop scheduled checks. The updater verifies the download and keeps the previous version for rollback.

## Features

- Chat, DMs, threads, voice, and screen sharing
- Editable themes, rounded layouts, and responsive panels
- Slash-command suggestions, attachment paste, and historical command lookup and copy when Discord provides the submitted details
- Thread controls, favorites, pinned DMs, and compact Discord links in chat and embeds
- **Plugins** below **Friends** offers the Vencord catalog, 11 native features, and a companion that bundles 147 upstream plugins. The companion uses a separate Discord login.

Some Discord features are still incomplete. Keep credentials, local profiles, and private message logs out of public issues.

## Source and license

S;C is licensed under [GPL-3.0-or-later](LICENSE). The source archive includes upstream attribution and build instructions. Use the `SC-source.zip` attached to the specific release for its matching source. See [Releasing](RELEASING.md) for the publication workflow.
