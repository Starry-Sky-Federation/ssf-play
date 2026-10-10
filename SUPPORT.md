# Getting help

I maintain SSF Play in my spare time. Answers are best effort and may take a few days. A report with the items listed below can usually be answered in one round; one without them usually cannot.

## Where to go

| For | Go to |
|---|---|
| A problem you can reproduce | [Bug report][new-bug] |
| How a game ran on your Mac, good or bad | [Compatibility report][new-compat] |
| A request for a game that has no app yet | The same form: [Compatibility report or game request][new-compat] |
| Purchases, download links and refunds; help for customers of the full edition | <support@ssf.network> |
| A security problem | [SECURITY.md](SECURITY.md). Not a public issue. |
| The setup guide, the notes on each game, troubleshooting | <https://play-docs.ssf.network> |

Issues are public. Do not put an order number, the e-mail address you paid with or a download link into one. A lost download link is sent again from <https://play.ssf.network/download>. The refund policy is part of the terms at <https://play.ssf.network/legal>.

## Before you report

1. **Read the end of the log** (see below). A line that starts with `[error]` usually names what failed.
2. **Press `F7`** in the Steam window and look for lines marked with a cross.
3. **A dialog titled "Rosetta 2 is required".** Run `softwareupdate --install-rosetta --agree-to-license` in Terminal, then open the app again.
4. **A dialog titled "SSF Play ... needs the runtime".** You opened one of the four small apps of the full edition and it found no runtime. Drag `SSF Play CS2.app` from the full edition's disk image into the same folder and open the app again. The `SSF Play CS2.app` of the Community Build does not count: its runtime leaves out what the other four apps need.
5. **Nothing seems to happen after the setup.** A freshly installed Steam first downloads its own update, about 1.4 GB. If the connection drops during that, Steam quits and the app shows "Steam did not start." Open the app again; the download continues where it stopped.
6. **The Steam window is black or does not react.** Press `F5` to reconnect, then `F2` to go through Steam's windows.
7. **A dialog says Steam is running with the settings of another app.** Choose "Restart Steam" to apply the settings of the app you opened. That interrupts downloads and closes a running game.
8. **A 32-bit game behaves oddly.** Turn the x87 accelerator off with `F8` twice and compare. Say in the report whether it made a difference.
9. **A graphics crash in a game.** macOS 27 is a beta, and the cause may be the system. Say in your report which graphics layer was in use: `F7` shows it. For `SSF Play MW2CR.app` and its third mission, see the known issues in the README first.

## What a useful report contains

The issue forms ask for each of these. This is where to find them.

### The app and the edition

The name of the app you opened, for example `SSF Play CS2`, and whether it came from the Community Build or from the full edition.

### The log

Each app writes its own log, named after the app:

```
~/Library/Logs/<App name>.log
```

For example `~/Library/Logs/SSF Play CS2.log`. The log is in English, whichever language the dialogs are in. Every start of an app begins with a line like `=== 2026-10-02 21:30:00 SSF Play CS2.app starts (appid 730) ===`, so the failing run is easy to find. The first-time setup writes to the same file.

To copy the last 200 lines to the clipboard, run this in Terminal (the quotes are needed because of the spaces in the name):

```bash
tail -n 200 ~/Library/Logs/"SSF Play CS2.log" | pbcopy
```

Look through the text before you post it. It contains paths with the name of your macOS account; replace that if you prefer, and remove anything else you consider private.

### The diagnostics panel (F7)

Press `F7` while the Steam window is in front. The panel is a dialog with one line for each part of the chain, each marked with a tick, a warning sign or a cross:

| Line | What it checks |
|---|---|
| Rosetta 2 | Whether Rosetta 2 is installed |
| Wine | Whether the runtime's Wine runs, and its version |
| 64-bit DX10/11 | The graphics layer of 64-bit games: DXMT or D3DMetal (the line then reads 64-bit DX10/11/12) |
| D3DMetal | Whether you have installed it |
| 32-bit DX10/11 | DXMT, or Wine's own wined3d |
| 32-bit DX9 | Always Wine's own wined3d |
| x87 accelerator | Whether it is there and works with the installed Rosetta |
| Windows environment (prefix) | Whether it has been set up |
| Steam | Whether Steam is installed in it |
| Privacy isolation | Whether drive `Z:` is closed off from your files |
| Steam window | Whether Steam's debug port answers |
| GPU | The chip macOS reports |

Take a picture of the panel (press Shift-Command-4, then Space, then click the dialog) and attach it to the report.

Some warning signs are expected:

- **D3DMetal: not installed.** It is optional.
- **On the Community Build, 32-bit DX10/11 and x87 accelerator.** That build leaves both out. They only matter for 32-bit games.

If the Steam window never appears, the panel can be opened from Terminal, provided the first-time setup got as far as copying the runtime (without a runtime the panel has nothing to check). For `SSF Play CS2.app` in Applications:

```bash
"/Applications/SSF Play CS2.app/Contents/MacOS/SSF Play CS2" --check-ui
```

For another app, put its name in both places.

### The version stamp

The runtime carries a small text file, `VERSION-ssfplay`, that says what it was built from. After the first-time setup it is here:

```bash
cat ~/Library/Application\ Support/SSF\ Play/wine/VERSION-ssfplay
```

It looks like this:

```
version=1.1.0
flavor=community
wine=wine-11.0
patches=<12 hex digits>
dxmt=v0.80-247-gfb45156
built=<date>
```

The full edition says `flavor=full` and has one more line, `x87sidecar=v1.7.0`. Paste the whole output.

If the setup did not get that far, the same file is inside the app that carries the runtime, at `SSF Play CS2.app/Contents/Resources/wine/VERSION-ssfplay`. The four small apps hold a copy of the stamp they expect as `Contents/Resources/runtime-stamp`.

### macOS and the chip

Apple menu, About This Mac shows the chip, the memory and the macOS version. On a beta the build number matters as well. In Terminal:

```bash
sw_vers
sysctl -n machdep.cpu.brand_string
```

The first command prints the macOS version and the build number (the `BuildVersion` line); the second prints the chip, for example `Apple M5 Max`.

### The graphics layer and the x87 accelerator

The title bar of the Steam window shows both: `Graphics: DXMT` or `Graphics: D3DMetal`, which is the graphics layer of 64-bit games, and, in the apps for 32-bit games, `x87 acceleration: on` or `off`. The labels are given here as the English window shows them; the other six languages translate them, but not the names DXMT and D3DMetal. After the graphics layer has been switched, the title bar keeps the old name until the app is opened again. The F7 panel shows the graphics layer as well, and always the current one.

If you have put anything into `~/Library/Application Support/SSF Play/<App name>.env`, include its content.

### What you did

The steps, in order, from opening the app to the problem; what you expected; what happened instead. The exact text of a dialog helps. A screenshot of the dialog is fine in any of the seven languages.

For a compatibility report, say how far you got (menu, a match, an hour of play) and describe how it ran in words. Please leave frame-rate numbers out: without a controlled measurement they cannot be compared. The number in the title bar of the Steam window that ends in `fps` is the refresh rate of that window, not of a game.

A request for a game that has no app yet goes into the same form: choose "A game that has no app yet" under App and "Does not apply (a request)" under Edition and Result, and name the game under Notes. You do not have to own the game to ask, and nothing about your Mac is needed. Requests are welcome; a request is noted, not promised.

## Not supported

- **Anti-cheat bans, and getting past anti-cheat.** SSF Play does not bypass, disable or modify any anti-cheat. Games with FACEIT or another kernel-level anti-cheat are not supported. The README says to try with a secondary account first; I cannot undo a ban.
- **Pirated or cracked copies.** SSF Play starts games from your own Steam library through the ordinary Steam client. Reports that involve anything else are closed.
- **Games you have not bought.** No game is included, and I cannot provide games, Steam accounts or Apple's Game Porting Toolkit.
- **macOS 28 and later.** The runtime depends on Rosetta 2, and Apple has said that from macOS 28 on, Rosetta stays only for some older games. I make no promise for those versions.
- **Intel Macs.** Apple Silicon only.
- **Problems that belong to Steam or to a game itself**, such as a game's servers, its own bugs or its account system.
- **A date for the app that is still in testing** (Grand Theft Auto V). There is none.

Reports from other Apple Silicon chips and from other builds of macOS 27 are welcome: I tested on a single machine, a MacBook Pro with an M5 Max.

[new-bug]: https://github.com/Starry-Sky-Federation/ssf-play/issues/new?template=bug_report.yml
[new-compat]: https://github.com/Starry-Sky-Federation/ssf-play/issues/new?template=compatibility_report.yml
