# S;C

A native Discord client for Windows with a compact interface, editable themes, and a focus on modest memory use. Built with Rust, Slint, and Skia.

**Unofficial, experimental, and unsigned.**

## Download

- [Application source](SC-source.zip)
- [Windows bootstrapper](SC-bootstrapper-windows.zip)
- [Archive checksums](SC-SOURCE-SHA256SUMS.txt)
- [Windows app releases](https://github.com/sso-coool/sc/releases)

The first downloadable app release is being prepared. The Windows installer targets Windows 10 and 11 on x64.

## Install and update

Extract the whole bootstrapper ZIP and run `Install-SC.cmd -Launch` once an executable release is available. Use `Start-SC.cmd` to check for stable releases before starting the app.

For scheduled updates, run `Enable-SCUpdates.ps1`. It checks every six hours by default and waits until S;C is closed. Add `-RestartWhenRunning` if you want it to restart S;C when an update is ready. You can use `-EveryHours 1` to check hourly or `-Disable` to stop scheduled checks. These updates can interrupt S;C calls when automatic restart is enabled.

The updater verifies the download and executable before replacing the app and retains the previous version for rollback.

## Features

- Chat, DMs, threads, voice, and screen sharing
- Editable themes, rounded layouts, and responsive panels
- Slash-command suggestions, attachment paste, and historical command details
- Thread controls, favorites, and pinned DMs

Some Discord features are still incomplete. Keep credentials, local profiles, and private message logs out of public issues.

## Source and license

S;C is licensed under [GPL-3.0-or-later](LICENSE). The source archive includes upstream attribution and build instructions. Each Windows release includes its matching source, SHA-256 checksums, and dependency notices. See [Releasing](RELEASING.md) for the publication workflow.
