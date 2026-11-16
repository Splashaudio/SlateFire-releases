# SlateFire

SlateFire is an app for firing video clips during live shows. Clips play full screen on a display, out of a Blackmagic device (UltraStudio, DeckLink), or both at once. You control it with the mouse, the keyboard, MIDI, OSC or RossTalk.

**Download the latest version from [Releases](../../releases/latest).**

## Mac

- Needs macOS 14 or later on Apple silicon (M1 or newer).
- Download `SlateFire-mac.zip`, unzip it, and move **SlateFire.app** to Applications.
- The first time you open it, macOS may say it's from an unidentified developer. Right-click (or Control-click) the app, choose **Open**, then **Open** again. Or go to System Settings ▸ Privacy & Security and click **Open Anyway**.

## Windows (coming soon)

- Needs Windows 10 or 11 (64-bit). Nothing else to install.
- Download `SlateFire-windows.zip` and unzip the whole folder. Keep all the files together and run **SlateFire.exe**.
- If Windows SmartScreen warns about an unknown publisher, click **More info** ▸ **Run anyway**.

## Blackmagic output

To output through a Blackmagic UltraStudio or DeckLink, install [Blackmagic Desktop Video](https://www.blackmagicdesign.com/support/family/capture-and-playback). On a Mac, also allow its system extension in System Settings ▸ Privacy & Security. Without it, SlateFire still works with display output only.

## Remote control

- **OSC** listens on UDP port 53200 by default.
- **RossTalk** listens on TCP port 7788 by default, for Ross switchers, DashBoard and Companion.
- **MIDI** is also supported.

Each app lists the full set of commands in Preferences ▸ Control.

## Licences

The Windows version includes [FFmpeg](https://ffmpeg.org) libraries, licensed under the LGPL.
