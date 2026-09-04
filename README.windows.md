# Dikte on Windows

Press `Ctrl+Space`, talk, press again: what you said is transcribed, cleaned
up and pasted where your cursor is.

## Requirements

Windows 10 or 11. The portable zip on the [releases page](../../releases)
carries everything else with it, and is x64. A checkout wants:

- **Python 3.11+** with **PyQt6** (`pip install PyQt6`; install.ps1 installs
  it when it is missing)
- **ffmpeg** for microphone capture: `winget install Gyan.FFmpeg`

## Installing

Download `Dikte-<version>-x64-portable.zip`, extract it, open the `dikte`
folder and run `Dikte.exe`. Nothing is installed and no administrator is
asked for. The application is signed with no certificate, so SmartScreen may
require **More info** → **Run anyway** on the first launch. Dikte can add its
own Start Menu, terminal command and start-at-sign-in entries; `dikte integrate
--remove` removes those entries, after which the extracted folder can simply
be deleted.

From a checkout instead:

```powershell
powershell -ExecutionPolicy Bypass -File install.ps1
```

This adds a **Dikte** entry to the Start Menu and a **`dikte`** command to the
terminal. Add `-Autostart` to also start it at sign-in; `-Uninstall` removes
all of it and leaves the repository and your settings alone.

To try it without installing anything:

```sh
python -m dikte
```

## First run

1. The tray icon appears and the Settings window opens.
2. Under **API and models**, download a local whisper model. On a Vulkan-capable
   x64 machine, the fork's ready-made Vulkan whisper.cpp package is fetched
   automatically; no compiler or Vulkan SDK is needed.
3. The shortcut defaults to `Ctrl+Space` and is changed under Shortcuts.
   While Dikte runs, Windows' own hotkey service (RegisterHotKey) listens for
   it: nothing to install and no permission to grant.

## What is different from Linux and macOS

- **Meeting recording (microphone + speakers) is not supported yet.** Windows
  does not offer what the speakers are playing as a capture device, so there
  is nothing to record the far side from. Everything else works, including
  transcribing audio and video files.
- **The shortcut is swallowed**: while Dikte holds `Ctrl+Space`, the focused
  application does not see it. This is how macOS behaves too, and unlike the
  Linux listener, which shares the key.
- No external tools for the clipboard or the key press: both go straight
  through the Windows API (the clipboard, SendInput).
- Settings live under `%APPDATA%\Dikte`, models and recordings under
  `%LOCALAPPDATA%\Dikte`.

## Performance

- On Windows x64 with a Vulkan driver, the Download buttons fetch this fork's
  Vulkan builds of whisper.cpp and llama.cpp. AMD Radeon, Intel and NVIDIA
  Vulkan devices can use the same packages. Machines without Vulkan keep the
  original upstream CPU download behavior.
- Setting Settings → API and models → **Threads** near your physical core
  count helps noticeably; the server's own default is 4.
- If speed matters more than accuracy, `ggml-small` and `ggml-base` are much
  faster; `ggml-large-v3-turbo-q5_0` transcribes best.

## Troubleshooting

- **Recording does not start:** does `dikte doctor` find ffmpeg, and does
  `dikte devices` list your microphone? `devices` also takes a fresh listing,
  which is what to run after plugging one in.
- **Nothing is pasted:** a normal-privilege process cannot type into an
  elevated (administrator) window; run Dikte elevated too, or paste by hand.
  The text lands on the clipboard either way.
- **The shortcut does nothing:** another application already holds the
  combination. Dikte says so in a tray notification when it asks for the key;
  pick a different one under Settings → Shortcuts.
