FFE COMPILER 1.0
================

Turn your own Final Fantasy Explorers ROM into a native PC game.

A small window with two buttons. Pick your .3ds file, press Build, and the
game's code is recompiled on your machine into a Vulkan program at up to 4x
resolution - then the HD textures and the enemy-range mod are added.
No game data ships with it. No saves either.

Download: FFE-Compiler-Installer-1.0.zip (96.5 MB) from the Releases page.
SHA256: 68346d2976ab10b916fa598150f80c2f505cfa97602dbf5b0c70ec316eee45e5

Linux only (Vulkan). Windows is not supported. Runs on a desktop or a Steam
Deck.


WHAT IT DOES
------------

Recompiles, not downloads
  The game's code is translated from your ROM into a native program when you
  press Build. Nothing pre-built is shipped or fetched, so the download
  contains no game code at all.

Looks like it should
  240 hand-made HD textures - menus, icons, grass, buildings, sky, mountains,
  character faces - plus sharp pixel-art sampling and 4x internal resolution,
  chosen in the window.

Two screens, one screen
  The 3DS bottom-screen map and HUD are folded into a movable panel over the
  top screen, so the game plays single-screen with a controller or trackpad.

Enemies that stay visible
  The monster render distance is extended from your own ROM's table, built
  during the install. Far-away goblins stop popping out of existence.


WHAT HAPPENS WHEN YOU PRESS BUILD
---------------------------------

  1. Your ROM is checked. Size, NCSD header and hash are printed, so you can
     see the tool has the file you meant.

  2. The tools are checked. Rust and a C compiler are looked for by name, and
     the window tells you what to install instead of failing halfway.

  3. The emulator and recompiler are put beside the output. They come with the
     zip; the game does not.

  4. Your ROM is recompiled. This is the long part - roughly eight of the
     twelve minutes.

  5. The native game and the emulator are built with Rust, against the local
     sources.

  6. Textures and the mod are installed, settings are written, and Play.sh
     appears.


WHAT YOU NEED
-------------

  * Final Fantasy Explorers (USA), dumped as
    "Final Fantasy Explorers (USA).3ds"
  * Rust - https://rustup.rs
  * A C compiler, and your distro's ALSA headers
  * About 5 GB of free disk space
  * Linux with Vulkan

Running it:

    unzip FFE-Compiler-Installer-1.0.zip
    cd "FFE Compiler installer"
    ./Install.sh

Without a window:

    python3 installer.py --headless \
      --rom "Final Fantasy Explorers (USA).3ds" \
      --out "$HOME/FFE Native"

Options: --no-sharp, --no-textures, --resolution 1..8


WHAT IS IN THE ZIP
------------------

96.5 MB, and none of it is the game:

  installer.py, Install.sh, README.txt   the window, the pipeline, the
                                         offline instructions
  bundle/zakuro/                         the emulator source (6.9 MB)
  bundle/3dsrecomp                       the recompiler binary (11 MB)
  bundle/mods/textures/                  240 HD texture mappings and 8
                                         animations, as PNGs (87 MB)
  bundle/tools/, bundle/overrides/       the enemy-range tool and the C
                                         overrides

Deliberately absent: no ROM, no game assets, no extracted tables, no
recompiled game code, no save files. Saves appear only in your install's own
user/ folder once you play. The HD artwork was drawn for this project and is
keyed by texture hash; it cannot be used to rebuild the ROM.


WHERE THINGS GO
---------------

Everything lands in the folder you choose, as one self-contained folder: the
game, the emulator it runs on, the mods, its settings and Play.sh. Delete the
folder and nothing else is left behind. It refuses to write into a folder that
already holds something of yours.

Settings live in user/config/zakuro/settings.toml and in the game's own
Settings window. Options in the installer: sharp textures, HD packs, 1x-8x 3D
resolution. You can change them later; remove a mod's PNGs to go back to the
original art without rebuilding.

About twelve minutes on a recent desktop CPU - most of it the recompiler and
the Rust build. The log shows "compiled N of 281" so you can see it working.


QUESTIONS
---------

Do I have to own the game?
  Yes. The installer cannot make anything without your dump: the game's code
  and the enemy-range table both come out of it. Nothing here works with a
  downloaded ROM of unknown origin.

Steam Deck?
  It builds and runs there - Vulkan, a controller and the inset panel work the
  same. Build it in Desktop Mode, then add Play.sh as a non-Steam game. A
  keyboard or trackpad is needed for the installer itself.

The build stopped. What now?
  The last lines of the log say why, in plain words. The two usual causes are
  a missing C compiler or ALSA headers (the libasound development package),
  and a full disk.


CREDIT AND LEGAL
----------------

An unofficial fan tool. Final Fantasy Explorers and Final Fantasy are
trademarks of Square Enix. No game data is distributed; you must own the game
to use this. Built on the fearkov/3dsrecomp port and the zakuro emulator, both
included as source in the zip.


HOW TO SHIP IT YOURSELF
=======================

The page is already committed in this folder's git repo (branch main, remote
origin = https://github.com/Jazzneo/FFE-Compiler-installer.git):

    cd github
    git push -u origin main

Then, on GitHub:

  1. Settings -> Pages -> Build and deployment -> Deploy from a branch ->
     main / (root). The .nojekyll file keeps the folder served verbatim.
     The page appears at https://jazzneo.github.io/FFE-Compiler-installer/

  2. Releases -> Draft a new release -> tag v1.0, then attach
     dist/FFE-Compiler-Installer-1.0.zip and dist/SHA256SUMS and publish.
     Keep the zip out of git: it is under GitHub's 100 MB per-file limit but
     well over the 50 MB warning line.

The page's download button points at:
https://github.com/Jazzneo/FFE-Compiler-installer/releases/latest/download/FFE-Compiler-Installer-1.0.zip
which resolves only once the release asset has exactly that filename.

If you would rather host the zip in the repo instead of a release, upload it
and change that one link in index.html.
