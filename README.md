# SSF Play

SSF Play is a set of macOS apps that run the Windows games in your own Steam library on an Apple Silicon Mac. Each app starts one game. Behind the apps is one shared runtime: Wine 11.0 running under Apple's Rosetta 2, with DXMT to translate Direct3D 10/11 to Metal, an accelerator for old 32-bit games, and a small native window that shows the Steam client. The games are not included: you sign in with your own Steam account and install what you own.

This repository holds no code. It is the place for this description, the downloads of the free Community Build, and problem reports. The apps' own programs are not open source; which parts are, and where their source is, is under [Open-source components and source](#open-source-components-and-source).

SSF Play is not affiliated with Valve, Activision, Take-Two Interactive, CodeWeavers or Apple, and is not endorsed by any of them. Please read [Limits](#limits) before you download or buy.

## What you need

| | Requirement | Note |
|:-:|---|---|
| 💻 | **A Mac with Apple silicon** | M1 or later. Intel Macs are not supported. |
| 🖥️ | **macOS 27** | Tested on the macOS 27 beta only. |
| 🔁 | **Rosetta 2** | If it is missing, the app installs it for you with one click. |
| 🌐 | **An internet connection** | On its first start Steam downloads about 1.4 GB. |
| 💾 | **Free disk space** | About 0.5 GB for the app, 1.4 GB for Steam, plus your games. |
| 👤 | **Your own Steam account** | And the games in it: no game is included. |

Nothing else has to be installed: no Homebrew, no Xcode, no extra tools.

## Compatibility

I tested on one machine: a MacBook Pro with an M5 Max, on the macOS 27 beta. The status column is a description of how each game played there. The only frame-rate figure is the one below the table. You must own each game on Steam; the names are here only to say what runs.

| App | Runs (you must own it on Steam) | Type | Direct3D goes through | Status |
|---|---|---|---|---|
| `SSF Play CS2.app` | Counter-Strike 2 | 64-bit, Direct3D 11 | DXMT, or D3DMetal if you install it yourself | Smooth |
| `SSF Play BO2.app` | Call of Duty: Black Ops II, campaign | 32-bit, Direct3D 11 | 32-bit DXMT, with the x87 accelerator | Smooth |
| `SSF Play BO2 MP.app` | Call of Duty: Black Ops II, multiplayer (bots and custom games) | 32-bit, Direct3D 11 | 32-bit DXMT, with the x87 accelerator | Smooth |
| `SSF Play BO2 Zombies.app` | Call of Duty: Black Ops II, Zombies | 32-bit, Direct3D 11 | 32-bit DXMT, with the x87 accelerator | Smooth |
| `SSF Play MW3.app` | Call of Duty: Modern Warfare 3 (2011), campaign and Special Ops | 32-bit, Direct3D 9 | Wine's wined3d on OpenGL, with the x87 accelerator | Smooth |

The statuses are what I saw while playing the games on the test machine.

Measured by the developer on a MacBook Pro (M5 Max): Call of Duty: Black Ops II (campaign, multiplayer and Zombies) and Call of Duty: Modern Warfare 3 run at about 90 frames per second at 2560×1600 with the highest graphics settings. This is what one machine showed, not a controlled benchmark; other Macs will differ.

A sixth app, for Grand Theft Auto V, is in testing and not released: no download contains it.

If you run one of the apps on another Mac, a [compatibility report](https://github.com/Starry-Sky-Federation/ssf-play/issues/new?template=compatibility_report.yml) helps: every status above comes from a single machine. The same form takes a request for a game that has no app yet.

## Editions

| | Community Build | Full edition |
|---|---|---|
| Price | Free | Shown as $10 and charged as ¥1,500 JPY, once; checkout shows the amount in your local currency |
| Apps | `SSF Play CS2.app` | All five apps in the table above |
| x87 accelerator | No | Yes |
| 32-bit DXMT | No | Yes |
| Disk image | `SSF-Play-Community-<version>.dmg` | `SSF-Play-<version>.dmg` |
| Where to get it | The [Releases](https://github.com/Starry-Sky-Federation/ssf-play/releases) of this repository | <https://play.ssf.network> |

- **Community Build.** One app, `SSF Play CS2.app`, and a `Read Me.txt`. Its runtime leaves out the x87 accelerator and the 32-bit DXMT; only the old 32-bit games need those, and their apps are not in this build. Use it to find out whether SSF Play runs on your Mac.
- **Full edition.** All five apps and the complete runtime, with a `Read Me.txt`. It is sold at <https://play.ssf.network>; the terms are at <https://play.ssf.network/legal>. It is not available from this repository.

Both disk images are signed with a Developer ID and notarised by Apple, and the notarisation ticket is stapled to each image. Each release here gives the SHA-256 of its disk image; compare it with the output of `shasum -a 256` on the file you downloaded.

All apps share one runtime, one Steam and one library. A Mac that has the full runtime is not downgraded by opening the Community Build; installing the full edition over the Community Build upgrades the runtime.

## Installing

You need an Apple Silicon Mac with macOS 27 (a beta at the time of writing, and the only version tested), your own Steam account, and the games in its library. Allow about 0.5 GB for the app that carries the runtime, the same again for the copy that the setup makes, about 1.4 GB for the Steam client, and the size of each game.

1. Install Rosetta 2 if it is missing: `softwareupdate --install-rosetta --agree-to-license` in Terminal. The apps check for it and show this command if it is not there.
2. Open the disk image and drag the apps onto the Applications shortcut. With the full edition, drag all five: only `SSF Play CS2.app` carries the runtime (about 0.5 GB). The other four are under 2 MB each and take the runtime from it on their first run.
3. Open any of the apps. macOS shows its usual question for an app downloaded from the internet; confirm it. The first-time setup then takes a few minutes and runs once. It copies the runtime to `~/Library/Application Support/SSF Play`, asks which graphics layer 64-bit games should use (the open-source DXMT is the default; press Return to keep it), creates an isolated Windows environment and installs Steam.
4. Sign in with your own Steam account in the window that opens, and install the game from your library. A freshly installed Steam first updates itself (about 1.4 GB). Once the game is installed, press `F9` or open the app again to start it.

The setup guide with every dialog, and notes on each game, are in the documentation: <https://play-docs.ssf.network>.

## Hotkeys

These work in the Steam window of an app. Every other key goes to Steam.

| Key | Action |
|---|---|
| `F9` | Start this app's game |
| `F8` twice | Turn the x87 accelerator on or off (32-bit games only; restarts Steam) |
| `F7` | Diagnostics panel: checks each part of the chain; from there you can switch the graphics layer or install D3DMetal from your own Game Porting Toolkit download |
| `F6` twice | Switch the graphics layer of 64-bit games between DXMT and D3DMetal (restarts Steam) |
| `F5` | Reconnect to Steam's interface |
| `F2` | Show the next Steam window (main window, sign-in, dialogs) |

Each app writes a log to `~/Library/Logs/<App name>.log`, for example `SSF Play CS2.log`. Settings for one app can be put in `~/Library/Application Support/SSF Play/<App name>.env` as `export NAME=value` lines.

## Languages

Dialogs and the window title are in one of seven languages: English, Simplified Chinese (简体中文), Traditional Chinese (繁體中文), Japanese (日本語), French (Français), German (Deutsch) or Spanish (Español). Without a choice they follow the first language in the system's language list, and fall back to English for any other language.

To choose, use the language menu: while the Steam window is in front, the app's menu bar has a menu with a globe as its title, with "Follow system" and the seven languages. The choice is kept in `~/Library/Application Support/SSF Play/language` and holds for all apps; the window title changes at once, the dialogs from the next one on.

The environment variable `SSFPLAY_LANG` (`en`, `zh-Hans`, `zh-Hant`, `ja`, `fr`, `de`, `es`) overrides both, for example as a line `export SSFPLAY_LANG=ja` in the app's `.env` file. The order is `SSFPLAY_LANG`, then the saved choice, then the system's list. The `.env` file is read only after the Rosetta check and the first-time setup, so those dialogs do not follow a language that is set there. While the variable is set, the menu marks the forced language and its entries cannot be chosen.

## How it works

- **Wine through Rosetta 2.** Wine lets Windows programs run on other systems without Windows. The runtime is Wine 11.0, built from the sources that CodeWeavers publishes for CrossOver 26.3.0, with six patches. It is an x86_64 program, as the games are, so Rosetta 2 translates it together with the game.
- **Direct3D on Metal.** Windows games draw with Direct3D; a Mac draws with Metal. DXMT translates Direct3D 10 and 11, comes with the apps and is the default. It is built from source, without changes, at a pinned commit of its main branch (`v0.80-247-gfb45156`). For 64-bit games there is a second choice, Apple's D3DMetal, which also supports Direct3D 12. It is not in any SSF Play download and the apps do not fetch it: you download Apple's Game Porting Toolkit with your own Apple ID, and the app installs D3DMetal from that file after showing you Apple's licence. Direct3D 9 uses Wine's own wined3d.
- **x87 accelerator** (full edition). Rosetta translates x87 floating-point code slowly, and old 32-bit game engines use a lot of it. athei's x87sidecar takes that translation over inside the 32-bit Wine processes. On the test machine a 32-bit x87 micro-benchmark loop takes 3419 ms without it and 215 ms with it (about 16 times faster, identical results). That is a micro-benchmark of x87 code, not a game frame rate; how much a game gains has not been measured, and 64-bit games do not go through it.
- **Steam window.** On macOS 27 Wine cannot draw Steam's own window; it stays black. The interface itself is rendered correctly by Steam's built-in browser, so a small native program reads the picture through that browser's debug port and sends mouse and keyboard input back. The game draws in its own window as usual. What this port is open to, and what not, is in [SECURITY.md](SECURITY.md).
- **One runtime, several apps.** The runtime, the Windows environment and Steam live in one data directory, `~/Library/Application Support/SSF Play`, and are set up once. Each app is a launcher with the settings of its game, so no extra tools have to be installed and no settings have to be changed by hand.

## Limits

- **No game content.** Nothing here contains game files. You need your own Steam account and you must own each game on Steam.
- **Trademarks.** Steam and Counter-Strike are trademarks of Valve Corporation. Call of Duty is a trademark of Activision Publishing, Inc. Grand Theft Auto is a trademark of Take-Two Interactive Software, Inc. SSF Play is not affiliated with them and not endorsed by them.
- **Anti-cheat.** Running a game through a compatibility layer carries a low but non-zero risk of a ban. SSF Play does not bypass, disable or modify any anti-cheat. FACEIT and other kernel-level anti-cheat systems are not supported. Try with a secondary account first.
- **Rosetta 2.** The runtime depends on Rosetta 2. Apple has said that from macOS 28 on, Rosetta stays only for some older games. I do not promise support for macOS 28 or later.
- **macOS 27 is a beta.** A graphics crash may come from the system and not from this project.
- **No Apple Game Porting Toolkit components.** Neither this repository nor any SSF Play download contains D3DMetal or any other Apple component, and the apps download none. If you install D3DMetal yourself, complying with Apple's licence is up to you.
- **Apple Silicon only.**

Also:

- I tested on a single machine. Other chips and other macOS versions are unverified.
- The Steam window uses Steam's own debug port, bound to `127.0.0.1` on a random port. Other programs running on the same Mac can find that port. See [SECURITY.md](SECURITY.md).
- The Windows environment is kept apart from your files (drive `Z:` and the user folders do not lead out of it), but it is not a sandbox.

## Open-source components and source

SSF Play is built on Wine and DXMT, which are open source. The apps' own programs are not: the launcher and setup scripts, the Steam window and the packaging are proprietary. They are not offered under an open-source licence, and the apps that contain them are covered by the terms at <https://play.ssf.network/legal>. See [LICENSE.md](LICENSE.md).

- **Wine and DXMT are under LGPL-2.1-or-later.** DXMT's `winemetal.so` links LLVM 15.0.7, which is under Apache-2.0 WITH LLVM-exception. x87sidecar is under the MIT licence.
- **What SSF Play changes and how it builds them is public**, as the LGPL asks: the six patches to Wine, and the scripts that build Wine, DXMT and the libraries Wine uses. DXMT's source is not changed.
- **Where the source is.** In the same place as the disk images, at `https://play.ssf.network/source/`, as three archives:
  - [`crossover-sources-26.3.0.tar.gz`](https://play.ssf.network/source/crossover-sources-26.3.0.tar.gz): the Wine sources as CodeWeavers publishes them, unchanged (sha256 `ac99c8ca4b3848f3e81784135f023df266b61c2345726ea55a50b3e030dd6872`);
  - [`dxmt-v0.80-247-gfb45156-src.tar.gz`](https://play.ssf.network/source/dxmt-v0.80-247-gfb45156-src.tar.gz): the unmodified DXMT source at commit [`fb45156`](https://github.com/3Shain/dxmt/tree/fb4515681daefb789a4d0f403c4bdbca88f3b3de), with its submodules;
  - [`ssf-play-lgpl-sources-1.0.0.tar.gz`](https://play.ssf.network/source/ssf-play-lgpl-sources-1.0.0.tar.gz): the patches and the build scripts, for version 1.0.0.

  The content of the third archive is also the repository [ssf-play-lgpl-sources](https://github.com/Starry-Sky-Federation/ssf-play-lgpl-sources). The three archives are also attached to every release of the Community Build in this repository, so the free download has its source next to it as well.
- **The libraries that come with the runtime.** The runtime carries libraries built with MacPorts. Those of eight projects are under the LGPL: gettext, gmp, gnutls, libiconv, libidn2, libtasn1, libunistring and nettle. The rest are under permissive licences, or offer one as a choice. The source archives of all of them, 20 files, are offered in the same place, unchanged, at `https://play.ssf.network/source/<file name>`. Where each archive comes from upstream, and the commits of the Portfiles the libraries were built with, are recorded in `share/licenses/BUNDLED-PORTS.txt` inside the disk image, together with the size and sha256 of each archive.

  <details>
  <summary>The 20 files, for version 1.0.0</summary>

  | Port | Licence the port declares | Source archive |
  |---|---|---|
  | brotli | MIT | [`brotli-1.2.0.tar.gz`](https://play.ssf.network/source/brotli-1.2.0.tar.gz) |
  | bzip2 | BSD | [`bzip2-1.0.8.tar.gz`](https://play.ssf.network/source/bzip2-1.0.8.tar.gz) |
  | freetype | FreeType License or GPL-2 | [`freetype-2.14.3.tar.xz`](https://play.ssf.network/source/freetype-2.14.3.tar.xz) |
  | freetype | FreeType License or GPL-2 | [`freetype-doc-2.14.3.tar.xz`](https://play.ssf.network/source/freetype-doc-2.14.3.tar.xz) |
  | gettext-runtime | LGPL-2.1+ or GPL-3+ | [`gettext-1.0.tar.xz`](https://play.ssf.network/source/gettext-1.0.tar.xz) |
  | gmp | LGPL-3+ | [`gmp-6.3.0.tar.bz2`](https://play.ssf.network/source/gmp-6.3.0.tar.bz2) |
  | gnutls-devel | LGPL-2.1+ and GPL-3+ | [`gnutls-3.8.13.tar.xz`](https://play.ssf.network/source/gnutls-3.8.13.tar.xz) |
  | libffi | MIT | [`libffi-3.4.8.tar.gz`](https://play.ssf.network/source/libffi-3.4.8.tar.gz) |
  | libiconv | LGPL-2+ or GPL-3+ | [`libiconv-1.19.tar.gz`](https://play.ssf.network/source/libiconv-1.19.tar.gz) |
  | libidn2 | LGPL-2.1+ or GPL-3+ | [`libidn2-2.3.8.tar.gz`](https://play.ssf.network/source/libidn2-2.3.8.tar.gz) |
  | libinotify | MIT | [`libinotify-kqueue-20240724.tar.gz`](https://play.ssf.network/source/libinotify-kqueue-20240724.tar.gz) |
  | libpcap | BSD | [`libpcap-1.11.0.tar.gz`](https://play.ssf.network/source/libpcap-1.11.0.tar.gz) |
  | libpng | libpng licence (zlib-style) | [`libpng-1.6.58.tar.xz`](https://play.ssf.network/source/libpng-1.6.58.tar.xz) |
  | libsdl2 | zlib | [`SDL2-2.32.10.tar.gz`](https://play.ssf.network/source/SDL2-2.32.10.tar.gz) |
  | libtasn1 | LGPL-2.1+ or GPL-3+ | [`libtasn1-4.21.0.tar.gz`](https://play.ssf.network/source/libtasn1-4.21.0.tar.gz) |
  | libunistring | LGPL-3+ or GPL-2+ | [`libunistring-1.4.2.tar.gz`](https://play.ssf.network/source/libunistring-1.4.2.tar.gz) |
  | nettle | LGPL-2.1+ | [`nettle-3.10.2.tar.gz`](https://play.ssf.network/source/nettle-3.10.2.tar.gz) |
  | p11-kit | Permissive (BSD-3-Clause upstream) | [`p11-kit-0.26.5.tar.xz`](https://play.ssf.network/source/p11-kit-0.26.5.tar.xz) |
  | zlib | zlib | [`zlib-1.3.2.tar.xz`](https://play.ssf.network/source/zlib-1.3.2.tar.xz) |
  | zstd | BSD or GPL-2 | [`zstd-1.5.7.tar.gz`](https://play.ssf.network/source/zstd-1.5.7.tar.gz) |

  </details>
- **The complete list** of components, with versions, licences and source addresses, is [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md). The paths it names below `patches/` and `scripts/` are in the third archive and in the source repository, not in this one. It was written before the library archives were copied to this site, and it still says that they are offered only by their projects and by the MacPorts mirror; those addresses remain valid, and the copies here are in addition to them. Every runtime carries the same file together with the licence texts, in `share/licenses/`: in a disk image that is `SSF Play CS2.app/Contents/Resources/wine/share/licenses/`, and after the first-time setup `~/Library/Application Support/SSF Play/wine/share/licenses/`.
- **You may modify the LGPL components** and use your own builds of them in the installed runtime, `~/Library/Application Support/SSF Play/wine`; the terms of use do not restrict that. The `README.txt` in the third archive gives the steps.

## Feedback

- **Problems, compatibility reports and requests for a game that has no app yet:** the [Issues](https://github.com/Starry-Sky-Federation/ssf-play/issues) of this repository. The forms ask for what is needed. A request is noted, not promised. [SUPPORT.md](SUPPORT.md) says where to find the log, the version stamp and the diagnostics panel, and what is not supported.
- **Purchases, download links and refunds:** <support@ssf.network>. Please do not put order details into a public issue.
- **Security problems:** <security@ssf.network>, not a public issue. See [SECURITY.md](SECURITY.md).

This repository takes reports and requests, not code: there is nothing here to send a pull request against. Changes to the LGPL parts belong to their own projects, or to the source repository named above. Conduct in the issues: [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Acknowledgements

- [CodeWeavers](https://www.codeweavers.com/), for publishing the CrossOver sources, and the [Wine](https://www.winehq.org/) project.
- [3Shain/dxmt](https://github.com/3Shain/dxmt): Direct3D 10/11 on Metal, and the [LLVM](https://llvm.org/) project, whose libraries DXMT's shader converter uses.
- [athei](https://github.com/athei): [x87sidecar](https://github.com/athei/x87sidecar) and the two Wine patches that go with it.
- [Gcenx](https://github.com/Gcenx): the macports-wine ports, and the llvm-mingw fixes behind one of the Wine patches.
- [MacPorts](https://www.macports.org/) and [llvm-mingw](https://github.com/mstorsjo/llvm-mingw).

SSF Play is a project of [ssf.network](https://ssf.network).
