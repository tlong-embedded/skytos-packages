# SkyTOS packages

Packages for SkyTOS, a small operating system for 32-bit x86 PCs (a Pentium II
is its real machine). The base system has no development tools and no DOS:
they come from here, through SkyTOS's package manager, `pkg`.

Each release is the whole package repository at one point in time: `INDEX`,
its signature `INDEX.SIG`, and a `.spk` file for each package. The latest
release is the current repository:

    https://github.com/tlong-embedded/skytos-packages/releases/latest/download/INDEX

Older releases stay, each a version of the whole repository.

## The packages

| Package | What it is |
|---|---|
| `dos` | Run DOS programs and games in a V86 machine |
| `dos-tests` | DOS test programs for dos: V86*.COM, the batch files, a CD's tracks |
| `tcc` | The Tiny C Compiler on SkyTOS, its headers, libraries, sources and examples |
| `gcc` | GCC 3.4.6 and binutils 2.17, hosted on SkyTOS |
| `gcc-kit` | GCC's sources and build commands, to rebuild cc1 on SkyTOS |

Each release's notes list the versions and sizes it has.

## Installing them

SkyTOS speaks HTTP, not HTTPS, so a Mac with the SkyTOS tree relays the
releases for it:

    make update-server PKG_FROM=github      # on the Mac

    pkg update 192.168.1.10                 # on SkyTOS: the Mac's address
    pkg install gcc
    pkg upgrade

The server fetches `INDEX` and `INDEX.SIG` from the latest release when SkyTOS
asks for them, and each package once, keeping them in `build/mirror`.

## Why they can be trusted

`pkg` takes no index without a good Ed25519 signature (`INDEX.SIG`) by a key
the system trusts (`/disk/SYS/PKG/KEYS`), and installs no package whose SHA-256
differs from the one in the index. The relay and this repository are not
trusted: a changed file is refused on SkyTOS. The packages here are signed by

    8f75f8d4c08627c0d5db341e96dfc6ce1a23e1d64dfc456e948d3f0734213830

## The format

`INDEX` is a line `SKYTOS-PACKAGES 1`, then a line for each package, its
fields separated by tabs: name, version, file, bytes, SHA-256, bytes
installed, packages it needs (`,`-separated, `-` for none), summary.

`INDEX.SIG` is one line: `ED25519`, the public key and the signature of
`INDEX`, in hex.

A `.spk` file is the magic `SKYPKG2\0`, a header (its own size, the body's,
the bytes installed, the number of files, then the name, version, summary
and dependencies), and a zlib stream: a table of the files (path, size,
mode, CRC-32) and then their contents, in that order.

## Licences

- SkyTOS's own programs, libraries and headers in these packages (`dos`, the
  test programs, the SDK's headers, `crt0.o`, `libposix.a`, the libc stubs):
  Copyright (c) 2026 Trey Long. All rights reserved.
- GCC 3.4.6 and binutils 2.17 (`gcc`, `gcc-kit`): the GNU General Public
  License, version 2 ([LICENSES/GPL-2.0.txt](LICENSES/GPL-2.0.txt)); parts of
  them, such as libiberty, the GNU Lesser General Public License.
- TCC (`tcc`): the GNU Lesser General Public License, version 2.1
  ([LICENSES/LGPL-2.1.txt](LICENSES/LGPL-2.1.txt)). The package carries its
  sources, SkyTOS's changes included, in `SDK/SRC`.

Each packages release links a `sources-` release with the sources of the GPL
and LGPL programs it carries: GCC, binutils and TCC as fetched from upstream,
SkyTOS's patches to them and the scripts that build them.
