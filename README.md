# S;C

A native Discord client for Windows, built with Rust, Slint and Skia. S;C is a modified [Yapfast](https://github.com/yapfast/yapfast) client with a compact interface, selectable themes and a focus on keeping memory use modest.

**Unofficial, experimental and unsigned.** S;C is not affiliated with Discord or endorsed by Yapfast. It does not have complete Discord feature parity. Discord's [Terms of Service](https://discord.com/terms) apply; compatibility and account safety are not guaranteed.

## Download

Get the Windows x64 build from [Releases](https://github.com/sso-coool/sc/releases). Windows 10/11 on x64 is the packaged target; there are no verified S;C macOS, Linux or ARM64 releases yet.

For manual installation, download `SC-windows-x64.zip` and `SC-SHA256SUMS.txt` from the same release. Verify the archive's SHA-256, extract the whole archive, and run `SC.exe` from the extracted directory. Keep its license and notice files alongside it.

```powershell
Get-FileHash .\SC-windows-x64.zip -Algorithm SHA256
```

Compare the hash with the `SC-windows-x64.zip` entry in `SC-SHA256SUMS.txt`. Hashes verify consistency with the published files; they are not a digital publisher signature. Windows may flag an unsigned executable. Do not disable Windows security protections to install it.

Quit any existing Yapfast or S;C instance before starting the new copy. S;C retains Yapfast's existing data and credential identifiers, so a separate application folder does **not** create an isolated account profile.

## Installer and updates

Download and extract `SC-bootstrapper-windows.zip` from the release. Keep its `.ps1`, `.psm1` and `.cmd` files together; the entry scripts require their shared module. They use Windows PowerShell 5.1 without extra modules. The installer targets `%LOCALAPPDATA%\Programs\SC` without administrator access:

```powershell
powershell -NoProfile -File .\Install-SC.ps1 -Repository sso-coool/sc -DesktopShortcut -StartMenuShortcut
```

To request an update explicitly:

```powershell
powershell -NoProfile -File .\Update-SC.ps1 -Repository sso-coool/sc -Restart
```

`-Tag v0.3.0.6` selects that specific release instead of the latest stable release. `-Restart` permits the updater to close the installed S;C process; finish a call or unsent edit before using it. The scripts verify the release archive and executable before replacing the application and retain the previous installation for rollback. They do not change Windows audio defaults, account data or credentials.

Launch through `Start-SC.cmd` to check for a newer stable release and apply it before opening S;C. If a check fails, the launcher opens the existing verified installation. A running S;C session is left alone by default. `Start-SC.cmd -NoUpdateCheck` skips that check. Opening `SC.exe` directly or using the installer-created app shortcuts also bypasses this launch-time check; scheduled checks work independently.

For automatic checks while signed in to Windows, enable the per-user scheduled task:

```powershell
powershell -NoProfile -File .\Enable-SCUpdates.ps1 -Repository sso-coool/sc
```

It checks every six hours, with the first check about five minutes after setup, and defers replacement while S;C is running. `-EveryHours 1` changes the interval. Add `-RestartWhenRunning` only if updates may interrupt calls and restart S;C. Disable scheduled checks with `Enable-SCUpdates.ps1 -Disable`. This uses Windows Task Scheduler; there is no continuously running updater process. The installer can also enable the schedule with `-AutomaticUpdates`.

The app's inherited Yapfast update page is not the S;C release channel. Use S;C's release tools to retain these changes.

## What is included

- Server chat, DMs, message actions, threads, voice and media features inherited from Yapfast.
- Native themes, editable colors, interface density, optional effects and automatic pane resizing.
- Inline or original slash-command input, optional arguments, autocomplete, attachment paste and favorite-bot suggestions. Hovering a command above a bot response can show its arguments when Discord supplies them.
- Permission-aware thread controls, forum tag filters and sorting, Mark as Unread, a searchable Blocked list, local Favorites and Pinned DMs.
- Explicit **System default** microphone and speaker choices.

Themes and resizing are in **Settings → Appearance**. Slash-input style is in **Settings → Messages**. See [the customization notes](docs/sc-custom.md) and [feature documentation](docs/discord/README.md) for details.

## Limits and testing

Activities, embedded web bot forms, media-gallery layouts, folder editing, onboarding and parts of server, privacy, voice and cosmetic settings remain incomplete. Favorites and pin edits stay on this device; importing received Discord data does not turn them into cloud-synchronized collections. Historical command details depend on Discord's response, and very long values may be clipped by the hover card. Name effects are native approximations.

Release notes identify the checks run for each build. Local fake-server tests and synthetic native UI tests do not certify every bot, account, media device or future Discord API change. Memory use varies with servers, messages, media and calls; no fixed RAM figure is promised for this fork.

The package contains no account credentials or session data. The app uses the existing OS credential store and `%LOCALAPPDATA%\Yapfast\Yapfast\data` for local application data. Never attach that directory, tokens or private message logs to a public issue.

## Source and license

S;C is licensed under **GPL-3.0-or-later**; see [LICENSE](LICENSE). It is based on Yapfast commit `ac3412e76784c9337f2ee85f0cabf48f0fa7edd2`, with S;C changes made in November 2026. Upstream attribution and third-party notices are retained.

Each binary release includes its matching `SC-source.zip`, `BUILD.json` provenance inside the application archive, and dependency notices in `THIRD-PARTY.json` and `licenses/`. Build from the matching source tag, not a later branch snapshot. The source includes pinned Skia preparation, native dependency configuration and packaging scripts; see [RELEASING.md](RELEASING.md), [native build documentation](docs/building.md) and [Windows notes](docs/windows-beta.md).

For development and release preparation, see [RELEASING.md](RELEASING.md). S;C's source, release assets and issue tracker are maintained at [sso-coool/sc](https://github.com/sso-coool/sc).
