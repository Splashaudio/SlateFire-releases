# SlateFire

SlateFire is an app for firing video clips during live shows. Clips play full screen on a display, out of a Blackmagic device (UltraStudio, DeckLink), or both at once. You control it with the mouse, the keyboard, MIDI, OSC or RossTalk.

**Download the latest version from [Releases](../../releases/latest).**

## Mac

- Needs macOS 14 or later on Apple silicon (M1 or newer).
- Download `SlateFire-mac.zip`, unzip it, and move **SlateFire.app** to Applications.
- The first time you open it, macOS may say it's from an unidentified developer. Right-click (or Control-click) the app, choose **Open**, then **Open** again. Or go to System Settings ▸ Privacy & Security and click **Open Anyway**.

## Windows

- Needs Windows 10 or 11 (64-bit). Nothing else to install.
- Download `SlateFire-windows.zip` (about 130 MB) and unzip the whole folder: right-click the zip, choose **Extract All**, and don't run the app from inside the zip. Keep all the files together and run **SlateFire.exe**.
- If Windows SmartScreen warns about an unknown publisher, click **More info** ▸ **Run anyway**.

## Blackmagic output

To output through a Blackmagic UltraStudio or DeckLink, install [Blackmagic Desktop Video](https://www.blackmagicdesign.com/support/family/capture-and-playback). On a Mac, also allow its system extension in System Settings ▸ Privacy & Security. Without it, SlateFire still works with display output only.

## Remote control

SlateFire can be controlled by OSC, RossTalk and MIDI. To turn each one on or off, click its light in the top right of the main window, or use Preferences ▸ Control. That is also where you change the ports.

### Choosing a clip: number or name

Wherever a command below says *n*, you can use the clip's **number** in the list or its **name**:

- `/cue/3/go` and `/cue/Intro/go` both work.
- Names are not case-sensitive, and spaces and punctuation are ignored, so `Paper_Skin` finds "Paper Skin".
- If two clips have the same name, the first one in the list is used. An unknown name is ignored.
- Clip numbers are positions in the list, so reordering the list renumbers the clips.

### OSC

SlateFire listens for OSC over **UDP, port 53200** by default.

| Address | Arguments | Action |
|---|---|---|
| `/cue/{n}/go` | — | Take clip *n* to air with its transition. If it's already on air, it restarts. |
| `/cue/{n}/cut` | — | Take clip *n* to air with a cut, whatever its transition |
| `/cue/{n}/preview` | — | Load clip *n* into Preview (the output doesn't change) |
| `/cue/{n}/stop` | — | Stop clip *n* if it's on air (fades to idle) |
| `/cue/{n}/level` | float (dB) | Set clip *n*'s audio level |
| `/cue/{n}/loop` | int 0/1 | Turn looping off or on for clip *n* |
| `/preview/go`, `/take` | — | Take the clip in Preview |
| `/stop`, `/black` | — | Master Stop: fade to idle. Send it again to cut straight away. |
| `/pause`, `/resume` | — | Pause or resume the clip on air |
| `/freeze` | int 0/1 | Freeze the output frame, or release the freeze |
| `/master/level` | float (dB) | Set the master audio level |
| `/live` | int 0/1 | Turn Live Mode off or on |

You can also send the clip as the first argument, for example `/cue/go 3`, `/cue/go "Intro"` or `/cue/level "Intro" -6`. Any `/clip/...` address works the same as `/cue/...`.

**Status and feedback.** SlateFire sends its reply to the address and port the query came from.

| Address | Reply |
|---|---|
| `/cue/{n}/state` | That clip's state, name, level, loop, elapsed and remaining time, colour, whether it's in Preview, and timecode. In Live Mode, the state can be `ready`. |
| `/master/state` | Master level, the clip on air, the clip count, the clip in Preview, the time remaining, whether the output is paused or frozen, the output status, whether Live Mode is on, and whether a clip is ready |
| `/state` | Everything above |
| `/subscribe` | Sends every change to you for 20 seconds. Send it again before the 20 seconds are up to keep it going. |
| `/unsubscribe` | Stops the updates |

### RossTalk

RossTalk works with Ross Video switchers (Carbonite, Acuity, Ultrix), DashBoard panels, Bitfocus Companion's RossTalk module and other RossTalk senders.

- SlateFire listens on **TCP, port 7788** by default. Several senders can be connected at once.
- Each command is one line of text ending in CR, LF or both. Commands are not case-sensitive. SlateFire doesn't send replies.
- The commands follow Ross XPression's, with clips in place of take items. XPression-style suffixes such as `TAKE 3:0:100` are ignored.

| Command | Action |
|---|---|
| `CUE n`, `FOCUS n`, `UPNEXT n` | Load clip *n* into Preview |
| `TAKE` | Take the clip in Preview |
| `TAKE n`, `SEQI n`, `GPI n` | Take clip *n* straight to air |
| `SEQO n` | Stop clip *n* if it's on air |
| `NEXT` | Take the clip after the one on air. If nothing is on air, take the first clip. |
| `UP`, `DOWN` | Move the Preview up or down the list |
| `CLRA`, `CLFB`, `STOP` | Master Stop. Send it again to cut. |
| `PAUSE`, `RESUME` | Pause or resume the clip on air |
| `FREEZE`, `UNFREEZE` | Freeze the output frame, or release it |

Preferences ▸ Control shows the last RossTalk command received. If the command wasn't used, it shows why, for example that no clip has that name.

To test from a Mac terminal:

```bash
printf 'TAKE 1\r\n' | nc -w 1 127.0.0.1 7788
```

### MIDI

SlateFire receives MIDI from every connected device, including devices plugged in after it starts. Each clip, Take and Master Stop can be triggered by a note, a control change (CC) or a program change. To set one up, click **Learn**: for a clip, it's in the clip editor; for Take and Master Stop, it's in Preferences ▸ Control. Notes and program changes trigger straight away. A CC triggers when its value rises from below 64 to 64 or above.

## Licences

The Windows version includes [FFmpeg](https://ffmpeg.org) libraries, licensed under the LGPL.
