# Third-party licences

This file lists the upstream components that the SSF Play runtime is built from: what is used, under which licence, and where its source is. It is a summary and not legal advice: for every component, its own licence text is what counts.

SSF Play's own programs (the launcher and setup scripts, the Steam window, the packaging) are not open source and are not a subject of this file. What the licences of the components below ask to be published is published: the patches, and the scripts that build Wine, DXMT and the libraries. Those scripts are under the MIT licence in the `LICENSE` file that comes with them; the patches carry the licence of what they change. See [Corresponding source](#corresponding-source).

**No released disk image and no published source archive contains a file of Apple's Game Porting Toolkit**: no `D3DMetal.framework`, no `libd3dshared.dylib`, none of its DLLs. The packaging and signing steps stop when they find one. D3DMetal can only enter a runtime on a user's own Mac, from a disk image that the user downloaded from Apple, after the app has shown Apple's licence from that image and the user has accepted it. The apps download nothing for this.

"Full" below is the full edition, `SSF-Play-<version>.dmg`. "Community" is the Community Build, `SSF-Play-Community-<version>.dmg`. "The source archive of this project" is `ssf-play-lgpl-sources-<version>.tar.gz`; it holds `patches/`, the three build scripts in `scripts/`, and this file.

## In the disk images

| Component | Version | Licence | Source | In |
|---|---|---|---|---|
| Wine, from the CrossOver sources | CrossOver 26.3.0 (Wine 11.0) | LGPL-2.1-or-later | <https://play.ssf.network/source/crossover-sources-26.3.0.tar.gz><br>That is an unchanged copy of the file CodeWeavers publishes at <https://media.codeweavers.com/pub/crossover/source/crossover-sources-26.3.0.tar.gz><br>sha256 `ac99c8ca4b3848f3e81784135f023df266b61c2345726ea55a50b3e030dd6872` | Full, Community |
| Patches to Wine | `patches/wine/` 0001 to 0006 | LGPL-2.1-or-later, as derivatives of Wine. `coop_proto.h`, which 0006 adds, is a verbatim copy of the protocol header of x87sidecar (MIT; see the x87sidecar row). | The source archive of this project. The origin and author of each patch are in [patches/README.md](https://github.com/Starry-Sky-Federation/ssf-play-lgpl-sources/blob/main/patches/README.md) and in the header of each patch file. | Full, Community |
| DXMT | `v0.80-247-gfb45156` (`git describe`): commit `fb4515681daefb789a4d0f403c4bdbca88f3b3de` of the main branch, unmodified, built from source by `scripts/build-dxmt.sh` | LGPL-2.1-or-later. Releases up to v0.80 were MIT; every later commit is LGPL. Code from other projects inside DXMT: see [below](#code-from-other-projects-inside-dxmt). | As one file with its submodules: <https://play.ssf.network/source/dxmt-v0.80-247-gfb45156-src.tar.gz> (see [Corresponding source](#corresponding-source))<br>Upstream: <https://github.com/3Shain/dxmt/tree/fb4515681daefb789a4d0f403c4bdbca88f3b3de> | Full: 64-bit and 32-bit. Community: 64-bit only. |
| LLVM, linked statically into DXMT's `winemetal.so` | 15.0.7: tag `llvmorg-15.0.7`, commit `8dfdcc7b7bf66834a761bd8de445840ef68e4d1a`, unmodified, built by `scripts/build-dxmt.sh llvm` | Apache-2.0 WITH LLVM-exception ("Apache License v2.0 with LLVM Exceptions"). The same licence file holds the older University of Illinois/NCSA licence, which still covers part of the code. Code from other projects inside its support library: see [below](#code-from-other-projects-inside-llvm). | <https://github.com/llvm/llvm-project/tree/llvmorg-15.0.7><br>or <https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.7/llvm-project-15.0.7.src.tar.xz> | Full, Community |
| x87sidecar | v1.7.0, unmodified, built from source | MIT. Copyright (c) 2025 Lifeisawful. The licence text is the MIT text printed [below](#code-from-other-projects-inside-dxmt); with the binary it is in the runtime as `x87sidecar/LICENSE`. | <https://github.com/athei/x87sidecar>, tag `v1.7.0` | Full |
| Libraries built with MacPorts | see the table [below](#libraries-built-with-macports) | see the table below | The source archive of each library is named in `BUNDLED-PORTS.txt` in the runtime; for the LGPL libraries also under [Corresponding source](#corresponding-source). Build recipes: <https://github.com/macports/macports-ports> with `patches/macports/` applied, and <https://github.com/Gcenx/macports-wine> | Full, Community |
| Rust crates of the Steam window | see [below](#rust-crates-of-the-steam-window) | see below | crates.io; the crates, their versions and their source addresses are listed in `rust-crates/CRATES.txt` in the runtime | Full, Community |

Wine's source tree bundles further libraries in its `libs/` directory, and the build compiles them into Wine's Windows modules: capstone, compiler-rt, FAudio, FluidSynth, gsm, jpeg, jxr, lcms2, ldap, mpg123, musl, png, tiff, tomcrypt, vkd3d, xml2, xslt and zlib. Each is under its own licence. Its licence files are in its directory in the Wine sources (for example `libs/png/LICENSE`), and `scripts/build-wine.sh install` copies them into the runtime as `wine-libs/<library>/`. Their source is part of the Wine source.

Vulkan is not part of the runtime: Wine is configured `--without-vulkan` and no MoltenVK is shipped.

### Code from other projects inside DXMT

DXMT's source tree holds code from other projects. It is compiled into the DXMT files above, and its source is part of the DXMT source. These are the notices found in the tree at the commit named above:

| Code | Place in the DXMT tree | Licence | Copyright notice |
|---|---|---|---|
| Parser for Direct3D shader bytecode (DXBCParser) | `libs/DXBCParser/` | MIT | Copyright (c) Microsoft Corporation. |
| Utility code derived from DXVK | `src/util/`; the notice is `src/util/dxvk.LICENSE` | zlib/libpng | Copyright (c) 2017 Philip Rebohle, Copyright (c) 2019 Joshua Ashton |
| MD5 and small-vector code from dxbc-spirv | `src/util/util_md5.cpp`, `src/util/util_md5.hpp`, `src/util/util_svector.hpp` | MIT | Copyright (c) 2025 Philip Rebohle |
| SHA-1 | `src/util/sha1/sha1.c`, `sha1.h` | Public domain | By Steve Reid |
| `tl::generator` | `include/tl/generator.hpp` | CC0-1.0 | Written in 2021 by Sy Brand |

The MIT licence, which applies to the two MIT rows with the copyright notices given there (and, with the notice given in its row above, to x87sidecar and to the header file that Wine patch 0006 takes from it):

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Four more things about the DXMT files:

- The DXMT build also produces `nvapi64.dll` and `nvngx.dll`. They are not in any disk image.
- DXMT's own Metal shaders are compiled with Xcode's Metal toolchain and embedded in its files. They are DXMT's code; nothing of Apple's Game Porting Toolkit is involved.
- The version resource inside DXMT's `d3d11.dll`, `d3d10core.dll` and `dxgi.dll` still reads "MIT License": the `version.rc` files of DXMT say so at this commit. DXMT's `LICENSE` and `LICENSE.OLD` files say that only the releases up to v0.80 were MIT and that DXMT is now LGPL-2.1-or-later; this file goes by them. Both files are in every runtime.
- Two files in the tree name an outside origin and carry no licence statement: `src/airconv/sha256.hpp` (it names Brad Conte as the author of the original code, from <https://github.com/B-Con/crypto-algorithms>, and Florian Ziesche for the C++ version) and `include/adt.hpp` (it refers to a Stack Overflow answer). Both are part of DXMT as its authors publish it and are compiled into the DXMT files.

### Code from other projects inside LLVM

LLVM's licence file says that code from other projects in its tree carries its own terms. Of that code, this is what the linker puts into `winemetal.so` from LLVM's support library (`llvm/lib/Support`):

| Code | Place in the LLVM tree | Licence | Copyright notice |
|---|---|---|---|
| Regular expressions | `regcomp.c`, `regexec.c`, `regerror.c`, `regfree.c` and their headers; the notice is `COPYRIGHT.regex` | BSD-style | Copyright 1992, 1993, 1994 Henry Spencer; Copyright (c) 1994 The Regents of the University of California. The full notice is in every runtime as `llvm/COPYRIGHT.regex`. |
| `strlcpy` for the regular expressions | `regstrlcpy.c` | ISC | Copyright (c) 1998 Todd C. Miller |
| UTF conversion | `ConvertUTF.cpp`, `ConvertUTF.h` | Apache-2.0 WITH LLVM-exception, with the notice of Unicode, Inc. below | Copyright 2001-2004 Unicode, Inc. |
| MD5 | `MD5.cpp` | Public domain | Written by Alexander Peslyak in 2001 |
| SHA-1 | `SHA1.cpp` | Public domain | |

BLAKE3 (`BLAKE3/`, CC0-1.0 or Apache-2.0) is in the same library; its licence file is in every runtime as well.

The notice of `regstrlcpy.c`:

> Copyright (c) 1998 Todd C. Miller <Todd.Miller@courtesan.com>
>
> Permission to use, copy, modify, and distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.
>
> THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

The notice of `ConvertUTF.cpp` and `ConvertUTF.h`:

> Copyright 2001-2004 Unicode, Inc.
>
> Disclaimer
>
> This source code is provided as is by Unicode, Inc. No claims are made as to fitness for any particular purpose. No warranties of any kind are expressed or implied. The recipient agrees to determine applicability of information provided. If this file has been purchased on magnetic or optical media from Unicode, Inc., the sole remedy for any claim will be exchange of defective media within 90 days of receipt.
>
> Limitations on Rights to Redistribute This Code
>
> Unicode, Inc. hereby grants the right to freely use the information supplied in this file in the creation of products supporting the Unicode Standard, and to make copies of this file in any form for internal or external distribution as long as this notice remains attached.

### Libraries built with MacPorts

The packaging step copies every MacPorts library that Wine loads into `lib/wine/x86_64-unix/` of the runtime, under the file name it is referenced by, rewrites its install name and the references to it to `@loader_path`, and signs it again. The libraries are otherwise unmodified, and they stay separate dynamic libraries.

Every runtime carries its own record of them, written at packaging time from the MacPorts installation that built them: `BUNDLED-PORTS.txt` gives the commits of the two ports trees and, for each port, its version, the licence it declares, its libraries, and its source archive with size, sha256 and download addresses. The licence files of each port's source archive are next to it, in a directory named after the port. That record is what counts for a given runtime; the table below is what versions 1.0.0 and 1.1.0 were built with (1.1.0 packages the same builds of Wine, DXMT and the libraries as 1.0.0). The licence column is the `license` field of the port; the licence files of each upstream project are authoritative.

| Files | Port | Version | Licence declared by the port |
|---|---|---|---|
| `libbrotlicommon.1`, `libbrotlidec.1`, `libbrotlienc.1` | brotli | 1.2.0 | MIT |
| `libbz2.1.0` | bzip2 | 1.0.8 | BSD |
| `libffi.8` | libffi | 3.4.8 | MIT |
| `libfreetype.6` | freetype | 2.14.3 | FreeType License or GPL-2 |
| `libgmp.10` | gmp | 6.3.0 | LGPL-3+ (GMP's own README: LGPL-3+ or GPL-2+) |
| `libgnutls.30` | gnutls-devel | 3.8.13 | LGPL-2.1+ for the library (the port also lists GPL-3+ for its tools, which are not shipped) |
| `libhogweed.6`, `libnettle.8` | nettle | 3.10.2 | LGPL-2.1+ in the port. Nettle's own manual says otherwise: LGPL-3+ or GPL-2+, and those are the licence files in its source archive. |
| `libiconv.2` | libiconv | 1.19 | LGPL-2+ or GPL-3+ |
| `libidn2.0` | libidn2 | 2.3.8 | LGPL-2.1+ or GPL-3+ in the port. The README of libidn2 says otherwise for the library: LGPL-3+ or GPL-2+, and those are the licence files in its source archive. |
| `libinotify.0` | libinotify (from Gcenx/macports-wine) | 20240724 | MIT |
| `libintl.8` | gettext-runtime | 1.0 | LGPL-2.1+ or GPL-3+ |
| `libp11-kit.0` | p11-kit | 0.26.5 | Permissive (BSD-3-Clause upstream) |
| `libpcap.A` | libpcap | 1.11.0 | BSD |
| `libpng16.16` | libpng | 1.6.58 | libpng licence (zlib-style) |
| `libSDL2-2.0.0` | libsdl2 | 2.32.10 | zlib |
| `libtasn1.6` | libtasn1 | 4.21.0 | LGPL-2.1+ or GPL-3+ |
| `libunistring.5` | libunistring | 1.4.2 | LGPL-3+ or GPL-2+ |
| `libz.1` | zlib | 1.3.2 | zlib |
| `libzstd.1` | zstd | 1.5.7 | BSD or GPL-2 |

The set is computed at packaging time from what Wine references, so it can change with the ports tree. Before a release, compare this table with `BUNDLED-PORTS.txt` of the staged runtime and correct it.

### Rust crates of the Steam window

The `SteamUI` binary, the native window that shows the Steam client, is a Rust program and links its crates statically. Which crates those are changes with the program, so no list is kept here. The packaging step asks `cargo metadata` for the crates that are linked into the binary for the platform it is built for, and writes the result into every runtime:

- `rust-crates/CRATES.txt`: every linked crate with its version, the licence it declares and the address of its source, and, by name, version and licence, the crates that are only used while building (procedural macros and build scripts);
- `rust-crates/<crate>-<version>/`: the licence files that the crate comes with.

Some crates are published without a licence file; `CRATES.txt` says so for each and gives the licence the crate declares and its repository. The Rust standard library, which is part of every Rust program, is under "MIT OR Apache-2.0".

## Used to build, not in the disk images

| Component | Version | Licence | Source | Use |
|---|---|---|---|---|
| llvm-mingw | the `llvm-mingw` port of Gcenx/macports-wine (20260826 when versions 1.0.0 and 1.1.0 were built) | LLVM: Apache-2.0 WITH LLVM-exception. mingw-w64 headers and runtime: permissive (ZPL-2.1, public domain, BSD-style). | <https://github.com/mstorsjo/llvm-mingw> | Cross-compiler for the Windows side of Wine and for DXMT's DLLs. The compiler support code that it links into Wine's Windows modules is under the permissive licences named here. DXMT's DLLs are linked `-static`: they also contain its C++ runtime (libc++, libc++abi and libunwind, which are part of LLVM and under its licence; the LLVM exception lifts the notice conditions of that licence for portions that end up in a binary as a result of compiling it) and parts of the MinGW-w64 runtime, whose notice asks to be passed on with such binaries and is in every runtime (see [Licence texts in the disk images](#licence-texts-in-the-disk-images)). |
| mingw-directx-headers | commit `9df86f2341616ef1888ae59919feaa6d4fad693d`, the submodule `include/native/directx` of DXMT | LGPL-2.1-or-later (DirectX headers of MinGW-w64: data structure definitions and short macros) | <https://github.com/misyltoad/mingw-directx-headers> | Headers for the macOS-side code of DXMT. Part of the DXMT source archive. |
| NVIDIA NVAPI headers | commit `d08488fcc82eef313b0464db37d2955709691e94`, the submodule `external/nvapi` of DXMT | MIT | <https://github.com/NVIDIA/nvapi> | Headers for DXMT's `nvapi64.dll`, which is built with DXMT's release options but is not in the disk images. Part of the DXMT source archive. If you ship `nvapi64.dll`, NVIDIA's MIT notice has to go with it. |
| Xcode and its Metal toolchain | the installed version | Apple's Xcode licence | <https://developer.apple.com/xcode/> | Compiler for the macOS side of Wine, LLVM and DXMT; the Metal compiler turns DXMT's own shaders into the form that DXMT embeds. |
| Meson, Ninja, CMake | the installed versions | Apache-2.0 (Meson, Ninja), BSD-3-Clause (CMake) | <https://mesonbuild.com>, <https://ninja-build.org>, <https://cmake.org> | Build tools for DXMT, LLVM and x87sidecar. |
| MacPorts base | 2.12.6 | BSD-3-Clause | <https://github.com/macports/macports-base/releases/tag/v2.12.6> | Builds the libraries above, inside the work directory. |
| MacPorts ports tree | The commit that `BUNDLED-PORTS.txt` in the runtime records. In the release build tree, when this file was last revised: `6d60d0d06890228ba70c53e20be727fcdb1b3080` | BSD-3-Clause (the Portfiles) | <https://github.com/macports/macports-ports> | Build recipes. |
| Patch to the ports tree | `patches/macports/0001-nettle-libffi-explicit-x86_64-build-triplet.patch` | BSD-3-Clause, as the Portfiles it changes | The source archive of this project | See [patches/README.md](https://github.com/Starry-Sky-Federation/ssf-play-lgpl-sources/blob/main/patches/README.md). |
| Gcenx/macports-wine | The commit that `BUNDLED-PORTS.txt` in the runtime records. In the release build tree, when this file was last revised: `123c1a96a26296918797ec6636b20d6e8a75ba83` | BSD-3-Clause (the Portfiles) | <https://github.com/Gcenx/macports-wine> | Provides the `llvm-mingw` and `libinotify` ports. Patch 0001 is adapted from its `llvm-mingw_fixes.patch`. |

## Fetched or supplied on the user's Mac, never shipped

| Component | Where it comes from | Licence |
|---|---|---|
| wine-mono | Not in the disk images and not installed by the first-time setup. Wine downloads it from WineHQ (<https://dl.winehq.org/wine/wine-mono/>) and installs it into the prefix when a Windows program needs .NET. | MIT for the Mono core; other parts under other free licences. See the licence files of the wine-mono release. |
| Steam client | `SteamSetup.exe`, downloaded from Valve's CDN by the first-time setup | Steam Subscriber Agreement. Not redistributed. |
| Games | The user's own Steam library | Each publisher's licence. No game file is included anywhere. |
| Rosetta 2 | Apple, `softwareupdate --install-rosetta` | Apple's macOS licence |
| Apple Game Porting Toolkit (D3DMetal) | Optional. The user downloads the disk image from developer.apple.com with their own Apple ID. | Apple's licence, shown by the app and accepted by the user before anything is copied. Never in a published source archive, never in a disk image, never downloaded by the apps. |

## Licence texts in the disk images

Every runtime carries the licence texts listed here in `share/licenses/`. In a disk image that is `SSF Play CS2.app/Contents/Resources/wine/share/licenses/`; after the first-time setup it is `~/Library/Application Support/SSF Play/wine/share/licenses/`.

| File in `share/licenses/` | What it is | Put there by |
|---|---|---|
| `THIRD_PARTY_LICENSES.md` | A copy of this file | the packaging step |
| `wine/LICENSE`, `wine/LICENSE.OLD`, `wine/COPYING.LIB`, `wine/AUTHORS` | Wine's copyright notice, the notice of the code from before Wine became LGPL, the GNU Lesser General Public License version 2.1, and the list of authors that the notices refer to | `scripts/build-wine.sh install`, from the Wine sources |
| `wine-libs/<library>/` | The licence files of the 18 libraries in Wine's `libs/` directory that are compiled into Wine, as they are at the top of each library's directory | `scripts/build-wine.sh install`, from the Wine sources |
| `dxmt/LICENSE`, `dxmt/LICENSE.OLD`, `dxmt/COPYING.LIB`, `dxmt/dxvk.LICENSE` | DXMT's copyright notice, the MIT notice of its releases up to v0.80, the GNU Lesser General Public License version 2.1, and the zlib notice of the code taken from DXVK | `scripts/build-dxmt.sh install`, from the DXMT sources |
| `llvm/LICENSE.TXT`, `llvm/COPYRIGHT.regex`, `llvm/BLAKE3-LICENSE` | The Apache License 2.0 with the LLVM exceptions (and the older University of Illinois licence), and the notices of the regex and BLAKE3 code in LLVM's support library | `scripts/build-dxmt.sh llvm` and `install`, from the LLVM sources |
| `mingw-w64/COPYING.MinGW-w64-runtime.txt` | The notices of the MinGW-w64 runtime | the packaging step, from llvm-mingw |
| `BUNDLED-PORTS.txt` | The record of the libraries built with MacPorts: ports tree commits, versions, source archives with sha256 and download addresses | the packaging step, from the MacPorts installation |
| `<port>/`, one directory for each port in `BUNDLED-PORTS.txt` | The licence files at the top of the port's source archive, and for a few ports the licence files of code inside the library that are kept further down in the archive | the packaging step, from the source archives that MacPorts built the ports from |
| `x87sidecar/LICENSE` | The MIT notice of x87sidecar | the packaging step, from the x87sidecar sources. Full; a Community Build that is packaged in the same run has it as well. |
| `rust-crates/CRATES.txt`, `rust-crates/<crate>-<version>/` | The crates linked into the Steam window, and the licence files they come with | the packaging step, from `cargo metadata` and the crate sources |

The packaging step stops when one of the files named here is missing, when a library in the runtime belongs to no port, or when no licence text is found for a port. The MIT notices for the code inside DXMT and two notices for code inside LLVM are in this file: [Code from other projects inside DXMT](#code-from-other-projects-inside-dxmt), [Code from other projects inside LLVM](#code-from-other-projects-inside-llvm).

## Corresponding source

Wine, the patches to it and DXMT are under the GNU Lesser General Public License, version 2.1 or later, and several of the MacPorts libraries are under a version of that licence. The source that corresponds to a released disk image is offered in the same place as the disk images, at `https://play.ssf.network/source/<file>`. `SHA256SUMS.txt` of the release gives the sha256 of the three files there.

1. **The patches and the build scripts**: `ssf-play-lgpl-sources-<version>.tar.gz`: <https://play.ssf.network/source/ssf-play-lgpl-sources-1.1.0.tar.gz> for version 1.1.0, and <https://play.ssf.network/source/ssf-play-lgpl-sources-1.0.0.tar.gz> for version 1.0.0. The patches and the three build scripts are the same in both. It holds the patches to Wine, the patch to the MacPorts ports tree, the scripts that build the libraries, Wine and DXMT (`scripts/setup-macports.sh`, `scripts/build-wine.sh`, `scripts/build-dxmt.sh`) with every build option, this file, and a `README.txt` that says how to build and how to put a build of your own into an installed runtime. The same files are in the repository <https://github.com/Starry-Sky-Federation/ssf-play-lgpl-sources>, at the tag `v<version>`.
2. **Wine**: `crossover-sources-26.3.0.tar.gz`, <https://play.ssf.network/source/crossover-sources-26.3.0.tar.gz>, with the checksum above: the tarball as CodeWeavers publishes it, unchanged. `scripts/build-wine.sh fetch` downloads it from CodeWeavers and checks it.
3. **DXMT**: `dxmt-<git describe>-src.tar.gz`, for this revision <https://play.ssf.network/source/dxmt-v0.80-247-gfb45156-src.tar.gz>. `scripts/build-dxmt.sh source` writes it. It is the upstream source at commit [`fb4515681daefb789a4d0f403c4bdbca88f3b3de`](https://github.com/3Shain/dxmt/tree/fb4515681daefb789a4d0f403c4bdbca88f3b3de), without any change, together with its two submodules at the commits which that revision pins: `external/nvapi` is <https://github.com/NVIDIA/nvapi> at `d08488fcc82eef313b0464db37d2955709691e94`, and `include/native/directx` is <https://github.com/misyltoad/mingw-directx-headers> at `9df86f2341616ef1888ae59919feaa6d4fad693d`. `scripts/build-dxmt.sh fetch` checks out exactly these.
   The archive leaves three files out: the documentation of NVIDIA's NVAPI SDK in `external/nvapi/docs` (a compiled help file, a Word document and a PDF). They are not used to build DXMT, and unlike the headers and samples of that submodule they come with no licence statement. `SOURCE.txt` in the archive names them, and says how to build from the archive should the upstream repository not be reachable.
   DXMT has made no release since v0.80, so upstream offers no source archive of this commit, and the archives that GitHub generates for a commit leave the submodules out.
4. **LLVM**, because it is linked into DXMT's `winemetal.so`: llvm-project at the tag `llvmorg-15.0.7`, without any change, from the URLs above. Its own licence does not ask for the source; it is named here because it is part of an LGPL library.
5. **The libraries built with MacPorts** whose port declares the LGPL, alone or as one of the licences to choose from. Each was built by its port, without any change of ours to its source, from the archive in this table, with the patches that its Portfile lists; the Portfiles are in the ports tree at the commit recorded in `BUNDLED-PORTS.txt` (`patches/macports/` changes the Portfile of nettle). The sha256 is the one the port checks. These archives, and those of the other libraries built with MacPorts, are also offered unchanged at `https://play.ssf.network/source/<file name>`, in the same place as the disk images; their projects and the MacPorts distfiles mirror offer them at the addresses given here and in `BUNDLED-PORTS.txt`. (Runtimes packaged before 3 October 2026 carry an earlier copy of this file, which says that these archives are not mirrored there; they were added afterwards.)

   | Library | Port and version | Source archive | sha256 | Addresses |
   |---|---|---|---|---|
   | `libgnutls.30` | gnutls-devel 3.8.13 | `gnutls-3.8.13.tar.xz` | `ffed8ec1bf09c2426d4f14aae377de4753b53e537d685e604e99a8b16ca9c97e` | <https://www.gnupg.org/ftp/gcrypt/gnutls/v3.8/gnutls-3.8.13.tar.xz><br><https://distfiles.macports.org/gnutls/gnutls-3.8.13.tar.xz> |
   | `libnettle.8`, `libhogweed.6` | nettle 3.10.2 | `nettle-3.10.2.tar.gz` | `fe9ff51cb1f2abb5e65a6b8c10a92da0ab5ab6eaf26e7fc2b675c45f1fb519b5` | <https://ftp.gnu.org/gnu/nettle/nettle-3.10.2.tar.gz><br><https://distfiles.macports.org/nettle/nettle-3.10.2.tar.gz> |
   | `libgmp.10` | gmp 6.3.0 | `gmp-6.3.0.tar.bz2` | `ac28211a7cfb609bae2e2c8d6058d66c8fe96434f740cf6fe2e47b000d1c20cb` | <https://ftp.gnu.org/gnu/gmp/gmp-6.3.0.tar.bz2><br><https://distfiles.macports.org/gmp/gmp-6.3.0.tar.bz2> |
   | `libidn2.0` | libidn2 2.3.8 | `libidn2-2.3.8.tar.gz` | `f557911bf6171621e1f72ff35f5b1825bb35b52ed45325dcdee931e5d3c0787a` | <https://ftp.gnu.org/gnu/libidn/libidn2-2.3.8.tar.gz><br><https://distfiles.macports.org/libidn2/libidn2-2.3.8.tar.gz> |
   | `libtasn1.6` | libtasn1 4.21.0 | `libtasn1-4.21.0.tar.gz` | `1d8a444a223cc5464240777346e125de51d8e6abf0b8bac742ac84609167dc87` | <https://ftp.gnu.org/gnu/libtasn1/libtasn1-4.21.0.tar.gz><br><https://distfiles.macports.org/libtasn1/libtasn1-4.21.0.tar.gz> |
   | `libunistring.5` | libunistring 1.4.2 | `libunistring-1.4.2.tar.gz` | `e82664b170064e62331962126b259d452d53b227bb4a93ab20040d846fec01d8` | <https://ftp.gnu.org/gnu/libunistring/libunistring-1.4.2.tar.gz><br><https://distfiles.macports.org/libunistring/libunistring-1.4.2.tar.gz> |
   | `libiconv.2` | libiconv 1.19 | `libiconv-1.19.tar.gz` | `88dd96a8c0464eca144fc791ae60cd31cd8ee78321e67397e25fc095c4a19aa6` | <https://ftp.gnu.org/gnu/libiconv/libiconv-1.19.tar.gz><br><https://distfiles.macports.org/libiconv/libiconv-1.19.tar.gz> |
   | `libintl.8` | gettext-runtime 1.0 | `gettext-1.0.tar.xz` | `71132a3fb71e68245b8f2ac4e9e97137d3e5c02f415636eb508ae607bc01add7` | <https://ftp.gnu.org/gnu/gettext/gettext-1.0.tar.xz><br><https://distfiles.macports.org/gettext/gettext-1.0.tar.xz> |

   The table is what versions 1.0.0 and 1.1.0 were built with; `BUNDLED-PORTS.txt` in a runtime is the record for that runtime, and it names the source archive of every other MacPorts library as well.

The file `VERSION-ssfplay` in a runtime records what it was built from: the Wine version, the first 12 hex digits of the SHA-1 of `patches/wine/series`, the DXMT revision (`dxmt=`, the `git describe` of the commit that was built; the letters after `-g` are the beginning of the commit hash) and the x87sidecar tag.

You may modify Wine, DXMT and the libraries and use your own builds in an installed runtime: the files of Wine are in `lib/wine/` of the runtime, those of DXMT there and, for 64-bit Direct3D, also in `lib/gfx/dxmt/`, and the libraries in `lib/wine/x86_64-unix/`. `README.txt` in the source archive of this project gives the steps.

If you distribute a build of your own:

- Offer all of the above next to the binary: the patches and build scripts at the revision you built from, the Wine sources, the DXMT source archive of the commit you built, and the source archives of the LGPL libraries that `BUNDLED-PORTS.txt` of your runtime names.
- If you build another DXMT revision (`DXMT_REF`), publish the source archive of that revision and correct the DXMT rows of this file, including the submodule commits and the notices listed under "Code from other projects inside DXMT". Read DXMT's `LICENSE` at that revision first.
- DXMT is built without patches. If you change its source, your archive must contain the changed source, and the changed files must say that you changed them and when. `scripts/build-dxmt.sh source` refuses a tree with local changes, and `verify` fails for a build that was made from one.
- Keep `share/licenses/` and the copyright notices in the runtime intact.
- Do not add terms that forbid modifying or reverse-engineering the LGPL components.
- Never include any Apple Game Porting Toolkit file.
