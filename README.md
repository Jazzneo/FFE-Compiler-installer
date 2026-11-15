# FFE Compiler

[![License: MIT](https://img.shields.io/badge/license-MIT-3da639)](LICENSE.txt)
[![Platform: Linux](https://img.shields.io/badge/platform-Linux-7fd3f7)](#requirements)
[![Requires: your own ROM](https://img.shields.io/badge/requires-your%20own%20ROM-e3bf6a)](#requirements)

**Rebuild Final Fantasy Explorers (USA) from the ROM you own into a native Vulkan game — with the HD
textures and the enemy-range mod — from one window.**

No game data. No saves. Your ROM, your machine, one button.

![The FFE Compiler window](screenshot.jpg)

## Download

Grab `FFE-Compiler-Installer-1.0.zip` from the [latest release](../../releases/latest)
(96.5 MB) and check it against `SHA256SUMS`.

```
736569cebd1fb817102b36089367742ca616004f9c85efd0406803a60042b56e  FFE-Compiler-Installer-1.0.zip
```

## What it is

A tkinter window — ROM picker, install folder, three options, Build and Play — wrapped around a
build pipeline. Pressing Build:

1. checks your ROM's size, NCSD header and prints its hash;
2. checks that Rust and a C compiler are there, and says what to install if not;
3. copies the emulator source, the recompiler and the tools beside the output folder;
4. recompiles the game's code from your ROM (most of the twelve minutes);
5. builds the native game and the emulator with Rust;
6. installs the HD texture packs, builds the enemy-range mod from your ROM's monster table, writes
   `settings.toml` and `Play.sh`.

The finished folder is self-contained: the game, the emulator it runs on, its mods, its settings,
its saves. Delete the folder and nothing is left behind.

## Requirements

- Your own `Final Fantasy Explorers (USA).3ds` (about 1 GB)
- Rust — <https://rustup.rs>
- A C compiler and your distribution's ALSA development headers
- About 5 GB of free disk space
- Linux with Vulkan (desktop, or a Steam Deck in Desktop Mode)

Windows is not supported. The installer is checked by running a full build from scratch:
12:31 on this machine's CPU, producing a 4.1 GB install whose `Play.sh --headless 30` ran 30 frames
at ~1161 MIPS and exited 0.

## Use

```sh
unzip FFE-Compiler-Installer-1.0.zip
cd "FFE Compiler installer"
./Install.sh
```

Without a window:

```sh
python3 installer.py --headless \
  --rom "Final Fantasy Explorers (USA).3ds" \
  --out "$HOME/FFE Native"
```

Options: `--no-sharp`, `--no-textures`, `--resolution 1..8`.

Once built, `Play.sh` in the install folder starts the game. Settings live in
`user/config/zakuro/settings.toml` and in the game's own Settings window; removing a mod's PNGs
reverts that texture set without rebuilding.

## What is in the zip

| | |
| --- | --- |
| `installer.py`, `Install.sh`, `README.txt` | the window, the pipeline, the offline instructions |
| `bundle/zakuro/` | the emulator source (6.9 MB) |
| `bundle/3dsrecomp` | the recompiler binary (11 MB) |
| `bundle/mods/textures/` | 240 HD texture mappings + 8 animations, as PNGs (87 MB) |
| `bundle/tools/`, `bundle/overrides/` | the enemy-range tool and the C overrides |
| `LICENSE.txt` | this project's MIT license, and the bundled components' |
| `CREDITS.txt` | everyone whose code and art this is built on |

Deliberately absent: the ROM, game assets, extracted tables, recompiled game code, and any save
files. The HD artwork was drawn for this project and is keyed by texture hash, so it cannot be used
to rebuild the ROM.

## Credits

zakuro and 3dsrecomp are fearkov's work (MIT); the emulator source here is a modified copy. The HUD
pack uses EB Garamond, Lato and Noto Sans Symbols 2 (OFL). The emulator and game build from 296
published Rust crates, each under its own license. The whole list — crates included — is in
[CREDITS.md](CREDITS.md).

## License

FFE Compiler is **MIT** licensed — [LICENSE.txt](LICENSE.txt). That covers the installer, the build pipeline,
this page, this project's changes to the emulator inside the zip, and the HD artwork drawn for it.

It does not cover:

- **the game.** Final Fantasy Explorers and Final Fantasy are trademarks of Square Enix. No ROM,
  game asset, extracted table or save file is distributed, and nothing here grants a right in the
  game — you must own it to use this.
- **zakuro and 3dsrecomp** — MIT, Copyright (c) 2026 fearkov; the emulator source in the zip is a
  modified copy of zakuro and keeps that notice at `bundle/zakuro/LICENSE`.
- **the HUD pack's fonts** — EB Garamond, Lato and Noto Sans Symbols 2, SIL Open Font License 1.1,
  with their texts beside them in the pack.
- **the 296 Rust crates** the emulator and game build from, each under its own license and listed
  crate by crate in [CREDITS.md](CREDITS.md).

[NOTICE](NOTICE) sets out the scope in full; [THIRD-PARTY.txt](THIRD-PARTY.txt) lists the bundled
components and what they are.

## Credit and legal

An unofficial fan tool. Final Fantasy Explorers and Final Fantasy are trademarks of Square Enix.
No game data is distributed; you must own the game to use this. Built on the
[fearkov/3dsrecomp](https://github.com/fearkov/3dsrecomp) port and the zakuro emulator, both
included as source in the zip.

---

## Publishing this as the GitHub Page

This folder and `dist/` beside it are ready to publish. Three things to change: the repo name, the
download link, and where the zip lives (a Release asset, not a committed file).

**1. Point the page at your repo.** The download button is
`https://github.com/Jazzneo/FFE-Compiler-installer/releases/latest/download/FFE-Compiler-Installer-1.0.zip`.
Replace both placeholders in `index.html`:

```sh
sed -i 's/YOUR-USERNAME/your-github-name/g; s/ffe-compiler/your-repo-name/g' index.html
```

**2. Put the page on `main` and turn Pages on.** In the repo: *Settings → Pages → Build and
deployment → Deploy from a branch → `main` / `/ (root)`*. The `.nojekyll` file keeps the folder
served verbatim.

**3. Attach the zip to a release.** It is 96.5 MB, under GitHub's 100 MB per-file limit but well over
the 50 MB warning line, so keep it out of git history and upload it as a release asset — the
`/releases/latest/download/<name>` link only resolves when the asset has exactly this name.

Web UI: *Releases → Draft a new release → tag `v1.0`*, then drag in
`FFE-Compiler-Installer-1.0.zip` and `SHA256SUMS`, and publish.

Command line, if you have the GitHub CLI:

```sh
mkdir ffe-compiler && cd ffe-compiler
git init -b main
cp -r "/run/media/system/game2/Final fantasy Explore/github/." .
git add -A
git commit -m "FFE Compiler 1.0: installer, page and download"
git remote add origin git@github.com:Jazzneo/FFE-Compiler-installer.git
git push -u origin main

gh release create v1.0 \
  "/run/media/system/game2/Final fantasy Explore/dist/FFE-Compiler-Installer-1.0.zip" \
  "/run/media/system/game2/Final fantasy Explore/dist/SHA256SUMS" \
  --title "FFE Compiler 1.0" \
  --notes "Installer-only download: no ROM, no game data, no saves. See README for requirements."
```

Nothing in this folder pushes anywhere by itself — the push and release are yours to run.
