# Releasing S;C

This guide describes publication of the modified Windows client, not an upstream Yapfast release. The first prepared build is **0.3.0.6**, based on Yapfast **0.3.0**. Use the full S;C version in its tag and release title; the upstream Cargo package version alone does not identify this fork's build.

## Repository layout

Publish the source tree at the repository root: `Cargo.toml`, `Cargo.lock`, `rust-toolchain.toml`, `.cargo/`, `src/`, `ui/`, `crates/`, `vendor/`, `scripts/`, `testing/`, `docs/`, `packaging/` and the existing license notices. Keep upstream attribution, and preserve the original README as `README.upstream.md` when replacing it with the S;C README. Include the installer/updater source and their tests in `scripts/` or a documented dedicated directory.

Do not upload the surrounding working directory. Exclude `target/`, `.local/`, downloaded toolchains, local build caches, personal install backups, diagnostic logs and any application-data or credential files. Use an explicit source manifest and review new files; an ignored file is not evidence that a package is safe.

Retain [LICENSE](LICENSE), existing copyright notices and the prominent modification notice in the README. The GPL's corresponding source includes the source and scripts needed to generate, install and modify the distributed program, subject to its stated exceptions. A diff alone is not a replacement for the matching source. See sections 1, 5 and 6 of the included license and [GPL v3](https://www.gnu.org/licenses/gpl-3.0.html).

## Build record

The verified local 0.3.0.6 binary used Rust **1.97.1**, target **x86_64-pc-windows-msvc**, default `desktop,discord` features, static CRT and `cargo build --release --locked`. It did not use `live-preview`.

The source already contains `scripts/prepare-windows-skia.py`, `nix/prepare-windows-skia.py`, pinned native sources/checksums in `nix/`, `vendor/webrtc-sys`, `scripts/release-env`, `scripts/package-windows.py` and shared notice/source packaging in `scripts/release_package.py`. Keep these, `Cargo.lock` and all patched `vendor/` sources. The Skia source pin is `6a8fe80eadfac737c12e64229a1b9530bc78f3d1`; the Windows WebRTC archive is LiveKit's `webrtc-51ef663`, with its checksum in `nix/flake.nix`. Make the corresponding dependency source available as required by its license. Do not claim byte-for-byte reproducibility without a clean rebuild comparison.

### Fresh native Windows development build

Install Rust 1.97.1, Visual Studio C++ tools and Windows 10/11 SDK, LLVM, NASM, Python 3 and Ninja. Start an x64 Visual Studio developer shell. Ensure `clang-cl`, `llvm-rc` (or SDK `rc.exe`), NASM and Ninja are on `PATH`; set `LIBCLANG_PATH` to LLVM's `bin` directory. For the conventional LLVM install:

```powershell
$env:LIBCLANG_PATH = 'C:\Program Files\LLVM\bin'
$env:Path = "$env:LIBCLANG_PATH;$env:Path"
python scripts/prepare-windows-skia.py
python scripts/prepare-windows-skia.py --print-env
```

Run the printed `$env:SKIA_BINARIES_URL = 'file:///…'` assignment in that same shell, then build:

```powershell
cargo build --release --locked
.\target\release\yapfast.exe --smoke-test
.\target\release\yapfast.exe --version
```

The preparation script builds the pinned static-CRT Skia archive under `.local/windows/skia/`; its `--jobs` option can limit compilation concurrency. Do not substitute rust-skia's ordinary Windows binary archive: its CRT and JPEG linkage differ. `python scripts/prepare-windows-skia.py --build` is the documented convenience route, but its final Cargo invocation does not add `--locked`.

### Shipping builds and packaging

Upstream documents native Windows builds as development builds because source paths can be embedded. Its supported release route remaps Rust/C paths in a Linux MSVC cross-build:

```sh
nix develop path:./nix#windows --command scripts/build-windows discord --no-installer
```

That route invokes `scripts/package-windows.py` and produces **Yapfast-branded** packages and upstream release metadata. It is a verified source reference, not a drop-in S;C publisher. For S;C, use the reviewed fork packaging workflow to retain the S;C filenames, source URL, manifest and bootstrapper contract. A native Windows release needs equivalent path-remapping/privacy verification rather than bypassing the upstream package guard. In either route, inspect the final executable for embedded private machine paths before publishing.

Build one native UI configuration at a time. Run affected unit tests, local fake-server round trips and offscreen native UI checks before the final standalone build. Record the exact release executable's version, startup smoke result, imported DLL check and SHA-256. Runtime memory figures must refer to that executable and state the workload; older measurements must not be relabeled as the new version's results.

## Release assets

Keep these asset names stable because the installer consumes them:

| Asset | Purpose |
| --- | --- |
| `SC-windows-x64.zip` | Complete Windows application directory, including `SC.exe`, `BUILD.json`, `SOURCE.txt`, license and dependency notices |
| `SC-SHA256SUMS.txt` | SHA-256 values for the final uploaded artifacts |
| `SC-source.zip` | Exact modified source corresponding to the executable, with build/install scripts |
| `SC-bootstrapper-windows.zip` | Complete bundle: `Install-SC.ps1`, `Update-SC.ps1`, `Start-SC.ps1`, `Enable-SCUpdates.ps1`, `SC.Update.psm1` and `.cmd` launchers |

`SC-build.json`, `SC-package-validation.json`, concise release notes and `SC-changes.patch` are useful supplementary assets. A patch supplements the source archive. Inspect public JSON before upload: publish provenance and test results, not local usernames, account identifiers, process IDs or machine-specific paths.

Optional sidecar verification follows this chain: release asset SHA-256 → downloaded ZIP → extracted `BUILD.json` → `SC.exe` hash; then compare `BUILD.json.source_sha256` with `SC-source.zip`. If a separate `SC-build.json` is uploaded, confirm its executable and source hashes agree with the in-package manifest. Validation reports describe checks; they are not signatures and do not replace verifying the artifacts themselves.

Regenerate checksums **after** all artifacts are final. Check ZIP CRCs, archive roots and paths, source completeness, executable hash against `BUILD.json`, and dependency notices. Installer validation should cover invalid hashes, missing assets, failed extraction, no-newer-release behavior, process/path checks, successful update and rollback without altering application data. Test any automatic launcher or schedule separately; document exactly when it checks and when it may restart S;C.

The current binaries and PowerShell scripts are unsigned. Publish that fact. SHA-256 checks and GitHub's asset digest detect inconsistent downloads but do not provide independent publisher authentication. Do not substitute an antivirus scan for source review or imply that a clean scan guarantees safety.

## Publish a complete release

Create a reviewed commit and tag such as `v0.3.0.6` in `sso-coool/sc`. Prepare a **draft** GitHub release, upload the complete asset set, verify names, sizes and digests, then publish it. Only publish a stable release when it is ready to be discovered by automatic update checks. Avoid replacing files under a published tag: ship a new version for corrected code or assets.

GitHub attaches a source snapshot for the tag automatically, but keep `SC-source.zip` when it is the source artifact referenced by the binary's manifest. Ensure both reflect the shipped source. [GitHub's release documentation](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases) explains the relationship between tags and attached assets.

Release notes should distinguish implemented functionality, synthetic validation and real-device verification. Keep known gaps visible; never claim complete Discord compatibility, a guaranteed memory ceiling, independent security auditing or code signing when those have not been established.

Publication is performed by the repository owner using their normal authenticated GitHub tools. Account credentials, PATs, signing secrets and local application data do not belong in commits, release artifacts, scripts or issue reports.
