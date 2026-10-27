# Installing Hamlib

This chapter ends with four working
commands — `rigctl`, `rigctld`, `rotctl`
and `rotctld` — and a version line you
can check. On a Debian or Ubuntu machine
the whole job is one package.

## The package install

Hamlib ships in the standard
repositories. The tools live in
`libhamlib-utils`, which pulls in the
library itself (`libhamlib4`) as a
dependency:

```bash
sudo apt update
sudo apt install libhamlib-utils
rigctl --version
```

Check: the version command answers with
the library's version and build. The
lab's copy — the same Ubuntu packages,
version 4.5.5 — said:

```text
rigctl Hamlib 4.5.5 Apr 05 11:43:08Z 2023 SHA=6eecd3
```

Your version may differ slightly by
distribution; anything in the 4.x series
behaves the way this guide describes.

Many readers will not install anything
at all: if you already run Fldigi,
WSJT-X, a logbook or a satellite
tracker, Hamlib most likely arrived
with it as a dependency, and the
commands above may already answer. Try
`rigctl --version` first; install only
what is missing.

## The lab's install, honestly

This guide's lab ran on a machine where
installing packages system-wide was not
an option, so it downloaded the very
same `.deb` packages (`libhamlib-utils`,
the library package, and two libraries
they depend on) and unpacked them into a
private folder, running the tools from
there. The bytes are identical to what
`apt install` would have laid down; only
the location differs. Nothing in the
chapters depends on that difference, and
`criteria.md` records it so the evidence
says exactly what was done.

## First look around

Three commands tour the new install
without touching any hardware:

```bash
rigctl --list | head
rigctl --list | wc -l
rotctl --list | grep -i dummy
```

The first shows the top of the model
list: model 1, the dummy; model 2, the
network backend; then the
manufacturers. The second counts the
lines — 284 on the lab machine, so
roughly 280 rig models. The third finds
the dummy rotator, also model 1, among
the fifty or so rotator models.

Check: `rigctl --help` lists the
options, and among them the four this
guide uses most: `-m` for the model
number, `-r` for the rig device (a
serial port, or a network address for
model 2), `-P` for the PTT type, and
`-v` for more verbose output — repeat
it, up to five times, for more detail
(chapter 11 lives there).

## Other routes in

- **Other distributions.** Fedora and
  friends package the same split: a
  `hamlib` package for the library and
  its utilities. The commands afterwards
  are identical.
- **Windows and macOS.** The project
  publishes builds for both, and both
  WSJT-X and Fldigi bring Hamlib along on
  those systems too. The tools' names
  and commands are the same; serial
  device names differ (chapter 10).
- **Building from source** is possible
  and documented by the project; it is
  how brand-new backends are tested. It
  is beyond this guide's scope — the
  packaged version is the one the lab
  verified — and chapter 13 points at
  projects that live closer to the
  source.

Nothing has been connected, keyed, or
transmitted. Next chapter makes the
first connection, to a radio made of
software.
