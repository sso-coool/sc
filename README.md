# S;C

A native Discord client for Windows with a compact interface, editable themes, and a focus on modest memory use. Built with Rust, Slint, and Skia.

**Unofficial, experimental, and unsigned.**

## Download

Get the Windows x64 app, the bootstrapper, the matching source, and checksums from [the latest release](https://github.com/sso-coool/sc/releases/latest). S;C targets Windows 10 and 11 on x64.

- For a portable copy, extract `SC-windows-x64.zip` and run `SC.exe`.
- For installation and updates, extract all of `SC-bootstrapper-windows.zip` and run `Install-SC.cmd -Launch`. Open S;C through `Start-SC.cmd` to check for a new stable release at startup.

## Automatic updates

Run `Enable-SCUpdates.ps1` to schedule a check every six hours. Updates wait until S;C is closed by default. Add `-RestartWhenRunning` to allow automatic restart, which can interrupt calls. Use `-EveryHours 1` to check hourly or `-Disable` to stop scheduled checks. The updater verifies the download and keeps the previous version for rollback.

## Features

- Chat, DMs, threads, voice, and screen sharing
- Editable themes, rounded layouts, and responsive panels
- Slash-command suggestions, attachment paste, and historical command details when Discord provides them
- Thread controls, favorites, pinned DMs, and compact Discord links in chat and embeds

Some Discord features are still incomplete. Keep credentials, local profiles, and private message logs out of public issues.

## Source and license

S;C is licensed under [GPL-3.0-or-later](LICENSE). The source archive includes upstream attribution and build instructions. Use the `SC-source.zip` attached to the specific release for its matching source. See [Releasing](RELEASING.md) for the publication workflow.
