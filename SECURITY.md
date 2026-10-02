# Security

## Reporting a problem

Please do not open a public issue for a security problem. Write to **security@ssf.network**.

Say what you found, how to reproduce it, the version (the content of `VERSION-ssfplay`; [SUPPORT.md](SUPPORT.md#the-version-stamp) says where it is), your Mac model and your macOS build. The log of the app (`~/Library/Logs/<App name>.log`) helps; remove your Steam account name from it first.

I maintain this in my spare time. Expect an answer within a few days. Fixes go into the next release; older releases are not updated.

Not in scope: vulnerabilities in Wine, DXMT, Steam or the games themselves, which belong to their own projects, and anti-cheat detection, which SSF Play does not work around.

## The Steam debug port

The Steam window of each app reads Steam's interface and sends input to it through the remote debugging port of Steam's built-in browser. Whoever can talk to that port can see Steam's interface and control it. It is the most sensitive part of the design.

What is done:

- The port listens on `127.0.0.1` only. It cannot be reached from the network.
- The launcher picks a new random port (20000 to 59999) for every start of Steam.
- The number is kept in `~/Library/Application Support/SSF Play/.cdp-port`, a file with mode 0600. The window reads it again before every connection. The launcher removes it when it stops Steam.
- Before the window lists or sends anything, it asks the port for `/json/version` and goes on only if the reply contains `Valve Steam`. The launcher makes the same check while it waits for Steam. A stale port file, or a port that another program has taken since, is therefore ignored.
- Steam's browser refuses WebSocket connections that carry an `Origin` header and HTTP requests whose `Host` header is not a local address. A web page open in a browser on the same Mac therefore cannot connect.
- Steam's own marker file, which would open the port on the fixed number 8080, is renamed out of the way.

What is not done, and cannot be:

- **No password and no token.** The listener belongs to Steam. Nothing can be put in front of it.
- **Other local programs can find the port.** Any program running on the same Mac, under your account or under another logged-in user's, can scan the local ports or read the number from Steam's command line. It can then read and control Steam's interface. Every Chromium-based application that runs with a debug port has the same exposure. The random port and the 0600 file make this harder; they do not prevent it.
- **The `Valve Steam` check is not authentication.** It guards against accidents. A local program that answers the way Steam does would pass it, and the window would then send that program the mouse and keyboard input meant for Steam.
- **The port stays open as long as Steam runs**, also after the window is closed.
- **Fallback to 8080.** If the port file is missing or unreadable, the launcher and the window try port 8080.
- The `Origin` and `Host` behaviour is that of the Chromium inside Steam, observed on one version of the client. An update of Steam could change it. If you see that it has changed, please report it as a security problem.

In short: the port is closed to the network and to web pages, and open to software that runs on the same Mac. If that matters to you, quit Steam when you are not playing.

## Other things to know

- **The Windows environment is not a sandbox.** Drive `Z:` points to an empty directory, and the user folders inside the Windows environment are plain directories that do not lead to your home folder. This keeps Windows programs from stumbling over your files. It does not confine them: a program under Wine is an ordinary process of your user account.
- **Downloads on your Mac.** The first-time setup downloads `SteamSetup.exe` over HTTPS from Valve's CDN and does not check it against a checksum of its own. After that, Steam updates itself. The apps' own programs download nothing else. Wine itself downloads wine-mono from WineHQ if a Windows program needs .NET; [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) names the address.
- **D3DMetal.** The apps never download it. They read only a disk image that you choose in a file dialog, show Apple's licence from that image, and copy files only after you accept. A disk image without a licence file is refused.
- **Signatures.** The released disk images are signed with a Developer ID and notarised by Apple. The sha256 of each disk image, and of the three source archives that are published with it, is published with each release. A disk image from anywhere other than the [Releases](https://github.com/Starry-Sky-Federation/ssf-play/releases) of this repository or <https://play.ssf.network> is not one of mine.
- **No telemetry.** The apps contact no server of SSF Play. The network traffic comes from the download of `SteamSetup.exe` during the first-time setup, from Steam and the games you run, and from Wine in the case named above.
