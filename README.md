# Eyewitness Dinosaur Hunter Patched
A patch and updated launch script for Eyewitness Dinosaur Hunter that works on windows 10 &amp; 11 No emulation required!

Fixes that make **Eyewitness Virtual Reality: Dinosaur Hunter 2.0**, the 2000 DK Multimedia CD-ROM re-release, run again on **Windows 10/11** and on
**Android handhelds through Winlator**. (Tested with Snapdragon Gen 2 and Adreno GPU but will likely work on anything running android.)

The game itself is unchanged. These fixes replace the parts of 1990s Windows that modern
systems no longer have, and they add a few quality of life features.

> [!IMPORTANT]
> **You must supply your own game ISO.** For copyright reasons, this release contains only the fixes:
> no ISO, no game files and no game program.
> - **Windows version:** rename your ISO to `Dinosaur Hunter.iso` and place it in `DK Eyewitness Dinosaur Hunter/Disc/`.
> - **Portable and Android versions:** extract your ISO's files into `Disc/DKDINO/` inside that version's folder.
>
> On the first start, the launcher copies the game program from your files and patches it, then converts the
> videos with the bundled ffmpeg so they show a picture. All of this is automatic, and the video conversion only happens once.
> Full steps are in [Adding your game ISO](#adding-your-game-iso).

## What was fixed?

| Problem on modern systems | Fix |
|---|---|
| The game draws with **WinG**, a Windows 95 graphics library that needs 16-bit code, which 64-bit Windows can't run | A new stand-in `WING32.DLL` does the same work with modern Windows drawing functions |
| At start-up the game rejects the sound system: *"The sound decompression software has not been installed properly"* | A 2-byte fix to `Dinosaur Hunter.exe`. Its built-in sound check used a wrong value that Windows 95 let through. The launchers apply it automatically to the copy of the game from your disc |
| 20 of the 21 videos use the **Indeo 3.2** codec, which modern Windows no longer has, so you got sound but no picture | On the first start, the launcher converts them to **Cinepak**, which Windows still supports, using the bundled **ffmpeg**. Size, frame rate and audio are unchanged. The converted copies go in `Game/DKCODE/VIDEOS`, and `WING32.DLL` plays them in place of the originals. Your ISO or extracted files are never changed (see [The video conversion](#the-video-conversion)) |
| On a page change, most of the old page stayed on screen. The page transitions run too fast on modern PCs | `WING32.DLL` redraws the whole new page once the transition has finished |
| No controller support | `dkhelper.exe` adds **Xbox controller** support (see below) |
| The game insists on a real CD-ROM drive | **Windows:** the launcher mounts your disc ISO automatically. **Portable and Android:** `WING32.DLL` presents a plain folder of disc files to the game as its CD, so nothing gets mounted |
| **Winlator:** the game froze on the DK logo screen, because Winlator refused to open sound the way the game asks | `WING32.DLL` opens the sound another way: the game's format, then a converted 16-bit format, then silence as a last resort. A watchdog also keeps the game going if sound stops responding |

### Controller

| Button | Action |
|---|---|
| Left stick | Move the pointer. It starts slow and precise, then speeds up the longer you hold it |
| A | Left click |
| B | Right click (can be set to Backspace in `dkhelper.ini`) |
| Start | Escape |

Mouse and keyboard work as they did on the original game.

---

## Release contents

The release has three versions. Each is a self-contained folder that includes everything it needs, ffmpeg
included. Once you've added your game ISO, there's nothing to install and no extra software needed.

| Version | For | What you supply | Where it goes |
|---|---|---|---|
| `DK Eyewitness Dinosaur Hunter` | Windows 10/11 | Your ISO, renamed to `Dinosaur Hunter.iso`. It's mounted automatically while you play | `Disc/` |
| `DK Eyewitness Dinosaur Hunter Portable` | Windows 10/11 | Your ISO's files, extracted. Nothing gets mounted, and it runs from anywhere, including a USB stick | `Disc/DKDINO/` |
| `DK Eyewitness Dinosaur Hunter Android` | Android handhelds via Winlator | Your ISO's files, extracted (Winlator can't mount ISOs) | `Disc/DKDINO/` |

---

## Adding your game ISO

You must supply your own ISO of the **Dinosaur Hunter 2.0** CD-ROM ( volume label `DKDINO`).
That's the edition these fixes were made and tested with; other editions haven't been tested.
If you have the CD but no ISO, you can rip the iso file from the disc with any disc-imaging program, [majorgeeks.com/imgburn](https://www.majorgeeks.com/files/details/imgburn.html)
For the Portable and Android versions, you can copy the files straight from the CD.

### Windows iso version: place your ISO in `Disc/`

1. Rename your ISO to exactly **`Dinosaur Hunter.iso`**.
2. Place it in the `Disc` folder. **Don't extract it**: this version uses the ISO file as it is.
   ```
   DK Eyewitness Dinosaur Hunter/
   └── Disc/
       └── Dinosaur Hunter.iso    ← your ISO
   ```
3. Double-click `Play Dinosaur Hunter.bat`.

### Windows Portable and Android versions: extract your ISO into `Disc/DKDINO/`

1. Extract **all** of your ISO's files into that version's `Disc/DKDINO/` folder:
   - **Windows 10/11:** double-click the `.iso` to open it as a drive. Select everything on it
     (Ctrl+A), copy it (Ctrl+C) and paste it into `Disc/DKDINO/` (Ctrl+V). Then eject the drive.
   - **7-Zip:** right-click the `.iso`, then choose **7-Zip → Extract files…** and pick `Disc/DKDINO/` as the destination.
2. Check the layout. The extracted folders must sit **directly** inside `DKDINO`, not one level deeper:
   ```
   Disc/
   └── DKDINO/
       ├── INTRO/            ← the launcher checks this folder exists
       ├── DKCODE/
       ├── DKSETUP/
       ├── DCR/, DER/, DNH/, …   ← the game's content folders
       ├── Autorun.exe, Autorun.inf, SETUP.EXE
       └── ReadMe.rtf, Order.wri
   ```
3. Start `Play Dinosaur Hunter.exe`.
   **Android:** do this once on your **PC** first, so the videos get converted (see below). Then copy the whole folder to the device.

If the launcher says *"The game disc folder is missing"*, `Disc/DKDINO/INTRO` wasn't found. Check the layout above.

### What happens on the first start

The release doesn't include DK's game program, `Dinosaur Hunter.exe`, or its companion files. On the first start, the
launcher copies them from the `DKCODE` folder of your ISO or extracted files into `Game/DKCODE`. It then **patches
the game automatically**, applying the 2-byte sound fix, so there's nothing else to do. Files already in `Game/DKCODE`,
such as the new `WING32.DLL` and your settings, are never overwritten.

### The video conversion

Your original disc's videos use the old **Indeo** codec, which modern Windows can't decode: without
conversion they play sound with no picture. So on the **first start** the launcher also:

1. Shows a small *"Preparing the videos…"* window with progress.
2. Converts each Indeo video to **Cinepak** with the bundled ffmpeg (`Game/ffmpeg`). This takes
   about 3 minutes on a modern PC (longer on older ones) on a typical PC, and only happens once.
3. Saves the converted videos in `Game/DKCODE/VIDEOS`, in the same folder layout as the disc.
   `WING32.DLL` then plays these copies instead of the originals. Your ISO or extracted files are never changed.

Later starts skip straight to the game. If a video fails to convert, the game still runs, and the
launcher tries that video again on the next start.

**Android:** the conversion runs on a PC, not on the device, where it would take far too long under
emulation. Before copying the Android folder to your device, double-click its `Play Dinosaur Hunter.exe`
once on your PC. It converts the videos and starts the game, and you can close the game straight away.
After that, you can delete `Game/ffmpeg` to save about 150 MB before copying.

**Saving space:** once `Game/DKCODE/VIDEOS` exists, `Game/ffmpeg` isn't needed any more and can be deleted in any version.

---

## What's in each version

### 1. `DK Eyewitness Dinosaur Hunter` (Windows 10/11)

```
DK Eyewitness Dinosaur Hunter/
├── Play Dinosaur Hunter.bat      ← double-click to play
├── README.txt                    ← player instructions
├── Disc/
│   ├── PUT YOUR ISO HERE.txt     ← reminder (can be deleted)
│   └── Dinosaur Hunter.iso       ← NOT INCLUDED: put your own disc ISO here
└── Game/
    ├── Launcher.ps1              ← mounts the ISO; on first start copies + fixes the game program
    │                                and converts the videos; points the game at the disc, starts it
    ├── ffmpeg/                   ← bundled ffmpeg (video conversion) + its licence
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← NOT INCLUDED: copied from your disc and fixed on first start
        ├── VIDEOS/               ← created on first start: the converted videos
        ├── Dinosaur Hunter.ini   ← game settings (paths filled in by the launcher)
        ├── WING32.DLL            ← new WinG replacement + page-change fix + converted-video playback
        ├── dkhelper.exe          ← controller support (starts and stops with the game)
        ├── dkhelper.ini          ← controller and graphics-fix settings
        └── TOURS/, Uninst.dll, … ← NOT INCLUDED: the rest of the disc's DKCODE files,
                                     copied from your disc on first start
```

**To play:** place your ISO in `Disc/` as `Dinosaur Hunter.iso`, copy the folder anywhere on your PC and double-click `Play Dinosaur Hunter.bat`.
While the game runs, a "DKDINO" virtual disc drive appears in File Explorer. Leave it alone while you play.
It goes away when you eject it or restart the PC.

### 2. `DK Eyewitness Dinosaur Hunter Portable` (Windows 10/11)

```
DK Eyewitness Dinosaur Hunter Portable/
├── Play Dinosaur Hunter.exe      ← double-click to play
├── README.txt                    ← player instructions
├── EyewitnessVirtualRealityPort.ico, desktop.ini  ← custom folder icon
├── Disc/
│   └── DKDINO/                   ← NOT INCLUDED: extract your ISO's files in here
│       └── PUT YOUR EXTRACTED GAME FILES HERE.txt  ← reminder (can be deleted)
└── Game/
    ├── ffmpeg/                   ← bundled ffmpeg (video conversion) + its licence
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← NOT INCLUDED: copied from your disc and fixed on first start
        ├── VIDEOS/               ← created on first start: the converted videos
        ├── Dinosaur Hunter.ini   ← game settings (paths filled in by the launcher)
        ├── WING32.DLL            ← WinG replacement + page-change fix + disc-folder-as-CD
        │                            + converted-video playback
        ├── dkhelper.exe          ← controller support
        ├── dkhelper.ini          ← controller, graphics-fix and [Disc] settings
        └── TOURS/, Uninst.dll, … ← NOT INCLUDED: the rest of the disc's DKCODE files,
                                     copied from your disc on first start
```

**To play:** extract your ISO's files into `Disc/DKDINO/`, copy the folder anywhere and double-click `Play Dinosaur Hunter.exe`. No virtual drive
appears and no PowerShell is used. The launcher points the game at `Disc/DKDINO`, and `WING32.DLL`
presents that folder to the game as its CD.

### 3. `DK Eyewitness Dinosaur Hunter Android` (Winlator)

```
DK Eyewitness Dinosaur Hunter Android/
├── Play Dinosaur Hunter.exe      ← point a Winlator shortcut at this
├── README.txt                    ← step-by-step Winlator setup
├── EyewitnessVirtualRealityAndroid.ico, desktop.ini  ← custom folder icon
├── Disc/
│   └── DKDINO/                   ← NOT INCLUDED: extract your ISO's files in here (Wine can't mount ISOs)
│       └── PUT YOUR EXTRACTED GAME FILES HERE.txt  ← reminder (can be deleted)
└── Game/
    ├── ffmpeg/                   ← bundled ffmpeg: used when you run the launcher once on a PC
    │                                (can be deleted afterwards, before copying to the device)
    └── DKCODE/
        ├── Dinosaur Hunter.exe   ← NOT INCLUDED: copied from your disc and fixed on first start
        ├── VIDEOS/               ← created on the first start on a PC: the converted videos
        ├── Dinosaur Hunter.ini
        ├── WING32.DLL            ← same as Windows, plus disc-folder-as-CD,
        │                            sound fallback, sound watchdog and start-up logging
        ├── dkhelper.exe          ← controller support
        ├── dkhelper.ini          ← settings, including [Sound], [Disc] and [Debug]
        └── TOURS/, Uninst.dll, … ← NOT INCLUDED: copied from your disc on first start
```

`Play Dinosaur Hunter.exe` is a small launcher, the same one the Portable version uses. On first start it copies
the game program from `Disc/DKDINO/DKCODE` and applies the sound fix. On a PC it also converts the videos; under Winlator it skips that step.
Every time, it fills in the game's paths and turns on
disc-folder-as-CD mode. It also tells Wine to use the included `WING32.DLL` instead of Wine's own,
starts the game, and waits until the game closes, so Winlator closes with it.

**To play on Android** (full details are in the folder's `README.txt`):
1. On a PC, extract your ISO's files into `Disc/DKDINO` (see [Adding your game ISO](#adding-your-game-iso)).
2. Still on the PC, double-click `Play Dinosaur Hunter.exe` once to convert the videos. Close the game when it
   appears. Optionally, delete `Game/ffmpeg` to save about 150 MB.
3. Install the official Winlator from [github.com/brunodev85/winlator/releases](https://github.com/brunodev85/winlator/releases).
4. Copy the folder into the device's **Download** folder. Winlator shows it as drive `D:`.
5. Create a container with **Screen Size 800x600**.
6. In the container, open the menu on `Play Dinosaur Hunter.exe` and choose **Create Shortcut**.
7. In Winlator's **Shortcuts** list, choose **Add to Home Screen** so the game launches straight from the Android launcher.

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
| `[Video]` | `Redirect` | 1 | Play the converted videos from `Game/DKCODE/VIDEOS` instead of the disc's |
| `[Disc]` *(Portable, Android)* | `FakeCD` | 1 | Present `Disc/DKDINO` to the game as its CD |
| `[Debug]` *(Android)* | `Trace` | 1 | Write `startup.log` and `launcher.log` |

---

## Building from source

The source code for the new components lives in `source/tools`:

| Folder / file | What it is |
|---|---|
| `wing32/wing32.c` | The WinG replacement DLL (32-bit). Build with TCC, then fix the export names: `tcc -shared -o WING32.DLL wing32.c -lgdi32 -luser32 -lkernel32` then `python fix_exports.py WING32.DLL` |
| `dkhelper/dkhelper.c` | Controller helper (64-bit): `x86_64-win32-tcc -o dkhelper.exe dkhelper.c -luser32` |
| `dkboot/dkboot.c` | The Portable and Android launcher: `tcc "-Wl,-subsystem=windows" -o "Play Dinosaur Hunter.exe" dkboot.c -luser32 -lkernel32 -lgdi32` |
| `patch_adpcm_check.py` | Applies the 2-byte sound-check fix to `Dinosaur Hunter.exe` by hand. The launchers do this automatically |
| `convert_videos.ps1` | Manual alternative to the automatic conversion: re-encodes the Indeo videos inside an unpacked disc folder to Cinepak with ffmpeg (`-Root <folder> -FFmpeg <ffmpeg.exe>`) |
| `ffmpeg-notice.txt` | The notice bundled with each copy of ffmpeg: version, licence and where to get the source |
| `make_iso.ps1` | Builds the disc ISO with Windows' built-in IMAPI2 (no extra software) |
| `unz_is3.py` | Unpacks InstallShield 3 `.Z` archives (Python port of zlib's `blast.c`) |

All components were built with [Tiny C Compiler](https://bellard.org/tcc/) 0.9.27.

---

## Legal

*Eyewitness Virtual Reality: Dinosaur Hunter* and all of its content, including the program, text, images,
sound and video, are © Dorling Kindersley. This project is an unofficial, non commercial fan made compatibility
fix and isn't affiliated with or endorsed by DK. The game disc isn't distributed here. Please use these
fixes with a copy of the game you own.

**FFmpeg** is bundled, unmodified, in each version's `Game/ffmpeg` folder to convert the videos. It's by the
[FFmpeg developers](https://ffmpeg.org) and licensed under the **GNU LGPL v3**. The build is the LGPL "shared" build of
FFmpeg 9.0 from [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds). Its source code is available from
[ffmpeg.org](https://ffmpeg.org/download.html), and the build scripts are in that repository. Each copy includes `LICENSE.txt` and a notice file.
