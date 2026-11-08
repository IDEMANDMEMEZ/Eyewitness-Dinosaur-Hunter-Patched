# Eyewitness Dinosaur Hunter Patched
A patched version of Eyewitness Dinosaur Hunter that works on windows 10 &amp; 11 out of the box!

Fixes that make **Eyewitness Virtual Reality: Dinosaur Hunter**, the 1996 DK Multimedia CD-ROM
(this copy is the 2000 "Dinosaur Hunter 2.0" re-release), run again on **Windows 10/11** and on
**Android based devices through Winlator**. (Tested with Snapdragon Gen 2 and Adreno GPU but will probably run fine on anything)

The gameplay itself is unchanged. These fixes replace the parts of 1990s Windows operating systems that modern
systems no longer have. Fixes also add a few quality of life features for couch gaming comfort.

## What was fixed?

| Problem on modern systems | Fix |
|---|---|
| The game draws with **WinG**, a Windows 95 graphics library that needs 16-bit code, which 64-bit Windows can't run | A new stand-in `WING32.DLL` does the same work with modern Windows drawing functions |
| At start-up the game rejects the sound system: *"The sound decompression software has not been installed properly"* | A 2-byte patch to `Dinosaur Hunter.exe`. Its built-in sound check used a wrong value that Windows 95 let through |
| 20 of the 21 videos use the **Indeo 3.2** codec, which modern Windows no longer has, so you got sound but no picture | The videos were re-encoded to **Cinepak**, which Windows still supports. Size, frame rate, frame count and audio are all unchanged |
| On a page change, most of the old page stayed on screen. The page transitions run too fast on modern PCs | `WING32.DLL` redraws the whole new page once the transition has finished |
| No controller support | `dkhelper.exe` adds **Xbox controller** support (see below) |
| The game insists on a real CD-ROM drive | **Windows:** the disc ships as an ISO that the launcher mounts automatically. **Portable and Android:** `WING32.DLL` presents a plain disc folder to the game as its CD, so nothing gets mounted |
| **Winlator:** the game froze on the DK logo screen, because Winlator refused to open sound the way the game asks | `WING32.DLL` opens the sound another way: the game's format, then a converted 16-bit format, then silence as a last resort. A watchdog also keeps the game going if sound stops responding |

### Controller

| Button | Action |
|---|---|
| Left stick | Move the pointer. It starts slow and precise, then speeds up the longer you hold it |
| A | Left click |
| B | Right click (can be set to Backspace in `dkhelper.ini`) |
| Start | Escape |

The mouse and keyboard keep working as normal.

---

## Release contents

The release has three versions. Each is a self-contained folder: nothing to install and no extra software needed.

| Version | For | Disc |
|---|---|---|
| `DK Eyewitness Dinosaur Hunter` | Windows 10/11 | ISO image, mounted automatically while you play |
| `DK Eyewitness Dinosaur Hunter Portable` | Windows 10/11 | Plain folder, nothing mounted. Runs from anywhere, including a USB stick OR CD Disc |
| `DK Eyewitness Dinosaur Hunter Android` | Android handhelds via Winlator | Plain folder, plus sound fixes for Winlator |

### 1. `DK Eyewitness Dinosaur Hunter` (Windows 10/11, ~672 MB)

```
DK Eyewitness Dinosaur Hunter/
├── Play Dinosaur Hunter.bat      ← double-click to play
├── README.txt                    ← player instructions
├── Disc/
│   └── Dinosaur Hunter.iso       ← the game disc (videos converted to Cinepak)
└── Game/
    ├── Launcher.ps1              ← mounts the ISO, points the game at it, starts the game
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← the game (2-byte sound-check patch)
        ├── Dinosaur Hunter.ini   ← game settings (paths filled in by the launcher)
        ├── WING32.DLL            ← new WinG replacement + page-change fix
        ├── dkhelper.exe          ← controller support (starts and stops with the game)
        ├── dkhelper.ini          ← controller and graphics-fix settings
        ├── TOURS/                ← the game's guided-tour scripts
        └── Uninst.dll, Uninst.ini, Dinosaur Hunter.png  ← files from the original install
```

**To play:** copy the folder anywhere on your PC and double-click `Play Dinosaur Hunter.bat`.
While the game runs, a "DKDINO" virtual disc drive appears in File Explorer. Leave it alone while you play.
It goes away when you eject it or restart the PC.

### 2. `DK Eyewitness Dinosaur Hunter Portable` (Windows 10/11, no mounting, ~666 MB)

```
DK Eyewitness Dinosaur Hunter Portable/
├── Play Dinosaur Hunter.exe      ← double-click to play
├── README.txt                    ← player instructions
├── EyewitnessVirtualRealityPort.ico, desktop.ini  ← custom folder icon
├── Disc/
│   └── DKDINO/                   ← the game disc as a plain folder (videos converted to Cinepak)
└── Game/
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← the game (2-byte sound-check patch)
        ├── Dinosaur Hunter.ini   ← game settings (paths filled in by the launcher)
        ├── WING32.DLL            ← WinG replacement + page-change fix + disc-folder-as-CD
        ├── dkhelper.exe          ← controller support
        ├── dkhelper.ini          ← controller, graphics-fix and [Disc] settings
        ├── TOURS/                ← the game's guided-tour scripts
        └── Uninst.dll, Uninst.ini, Dinosaur Hunter.png  ← files from the original install
```

**To play:** copy the folder anywhere and double-click `Play Dinosaur Hunter.exe`. No virtual drive
appears and no PowerShell is used. The launcher points the game at `Disc/DKDINO`, and `WING32.DLL`
presents that folder to the game as its CD.

### 3. `DK Eyewitness Dinosaur Hunter Android` (Winlator, ~666 MB)

```
DK Eyewitness Dinosaur Hunter Android/
├── Play Dinosaur Hunter.exe      ← point a Winlator shortcut at this
├── README.txt                    ← step-by-step Winlator setup
├── Disc/
│   └── DKDINO/                   ← the game disc as a plain folder (Wine can't mount ISOs)
│       └── .windows-label        ← gives the folder the volume label "DKDINO" under Wine
└── Game/
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← same patched game as the Windows version
        ├── Dinosaur Hunter.ini
        ├── WING32.DLL            ← same as Windows, plus disc-folder-as-CD,
        │                            sound fallback, sound watchdog and start-up logging
        ├── dkhelper.exe          ← controller support
        ├── dkhelper.ini          ← settings, including [Sound], [Disc] and [Debug]
        └── TOURS/ …
```

`Play Dinosaur Hunter.exe` is a small launcher. It fills in the game's paths and turns on
disc-folder-as-CD mode. It also tells Wine to use the included `WING32.DLL` instead of Wine's own,
starts the game, and waits until the game closes, so Winlator closes with it.

**To play on Android** (full details are in the folder's `README.txt`):
1. Install the official Winlator from [github.com/brunodev85/winlator/releases](https://github.com/brunodev85/winlator/releases).
2. Copy the folder into the device's **Download** folder. Winlator shows it as drive `D:`.
3. Create a container with **Screen Size 800x600**.
4. In the container, open the menu on `Play Dinosaur Hunter.exe` and choose **Create Shortcut**.
5. In Winlator's **Shortcuts** list, choose **Add to Home Screen** so the game launches straight from the Android launcher.

**Troubleshooting:** each start writes `launcher.log` and `startup.log` in `Game/DKCODE`. They record
the disc check, sound and video activity, and any error messages. Turn them off with `Trace=0` under `[Debug]` in `dkhelper.ini`.

---

## Settings (`Game/DKCODE/dkhelper.ini`)

| Section | Key | Default | Meaning |
|---|---|---|---|
| `[Controller]` | `Enabled` | 1 | Controller support on/off |
| | `B` | `rightclick` | What B does: `rightclick` or `backspace` |
| `[Graphics]` | `FixTransitions` | 1 | Redraw the whole page after a page change |
| | `Log` | 0 | Write `wing32.log` with every drawing call (very detailed) |
| `[Sound]` *(Android)* | `Fallback` | 1 | Open sound another way if Winlator refuses the game's way |
| | `Watchdog` | 1 | Carry on silently if sound stops responding |
| `[Disc]` *(Portable, Android)* | `FakeCD` | 1 | Present `Disc/DKDINO` to the game as its CD |
| `[Debug]` *(Android)* | `Trace` | 1 | Write `startup.log` and `launcher.log` |

---

## Building from source

The source code for the new components lives in `source/tools`:

| Folder / file | What it is |
|---|---|
| `wing32/wing32.c` | The WinG replacement DLL (32-bit). Build with TCC, then fix the export names: `tcc -shared -o WING32.DLL wing32.c -lgdi32 -luser32 -lkernel32` then `python fix_exports.py WING32.DLL` |
| `dkhelper/dkhelper.c` | Controller helper (64-bit): `x86_64-win32-tcc -o dkhelper.exe dkhelper.c -luser32` |
| `dkboot/dkboot.c` | The Portable and Android launcher: `tcc "-Wl,-subsystem=windows" -o "Play Dinosaur Hunter.exe" dkboot.c -luser32 -lkernel32` |
| `patch_adpcm_check.py` | Applies the 2-byte sound-check patch to `Dinosaur Hunter.exe` |
| `convert_videos.ps1` | Re-encodes the Indeo videos to Cinepak with ffmpeg |
| `make_iso.ps1` | Builds the disc ISO with Windows' built-in IMAPI2 (no extra software) |
| `unz_is3.py` | Unpacks InstallShield 3 `.Z` archives (Python port of zlib's `blast.c`) |

All components were built with [Tiny C Compiler](https://bellard.org/tcc/) 0.9.27.

---

## Legal

*Eyewitness Virtual Reality: Dinosaur Hunter* and all of its content, including the program, text, images,
sound and video, are © Dorling Kindersley. This project is an unofficial, fan made non commercial compatibility
fix and isn't affiliated with or endorsed by DK. Please use it with a copy of the game you own.
