# Hamlib for Mere Mortals

Almost every modern amateur radio has
a port around the back, and through
that port a computer can do nearly
everything the front panel can: read
and set the frequency, change the
mode, and press the transmit switch.
The trouble is that every maker
invented a different language for it.
**Hamlib** is the free library that
learned all of those languages: about
three hundred **backends**, one per
radio family, behind a single set of
commands — so the logbook, the
digital-mode program and the satellite
tracker can all drive the same rig
without each learning its dialect.

This guide explains how that works and
teaches the four tools that come with
the library: **rigctl**, which drives
a radio from a terminal; **rigctld**,
which turns one radio into a network
service many programs can share;
**rotctl** and **rotctld**, their
twins for antenna rotators. Every
procedure is practised on Hamlib's
**dummy rig** — a backend that
pretends to be a radio — so the whole
book can be worked through with no
radio connected, no licence needed,
and nothing transmitted.

The rule that governs everything here
is stated in chapter 1 and repeated
in chapter 14: **the software can key
a real transmitter — stay inside your
licence, your band edges, and dummy
loads while you learn.** The lab
obeyed the strictest reading of it.
It never connected a radio at all;
everything below happened to a radio
made of software.

The lab behind this guide ran the
real thing: **Hamlib 4.5.5**, from
Ubuntu's own packages, exercised
through its own tools. Against the
dummy backend the lab listed the
models, dumped the capabilities,
held sessions that read and set
frequency and mode, asked after
VFOs and signal levels, and keyed
the dummy's PTT — after discovering,
and diagnosing from the error trace,
that the dummy refuses to key until
its PTT type is named. It started
**rigctld** and drove it with two
separate rigctl clients, which
watched each other's changes because
the state lives in one place, and
then spoke to the daemon raw, with
netcat, in its own plain-text
protocol — including the deliberate
nonsense command that the protocol
answers with silence. And it ran
**rotctl** against the dummy
rotator, where it caught the dummy
accepting positions it never moves
to, with the verbose trace as proof.

What the lab could not do, it did
not fake. It had no radio and no
serial hardware, so real rigs, real
cables, baud rates, permissions on
real devices, and the Fldigi and
WSJT-X settings screens are taught
from the documentation and labelled
as such wherever they appear; the
lab verified everything those
programs stand on, because they are
clients of the same protocol the lab
drove by hand. `criteria.md` keeps
the full ledger of what was run and
what was only read.

## What this guide covers

- [**What Hamlib is**](what-hamlib-is.md) —
  the translation problem, CAT, the
  backends, and the safety rule that
  is part of the tool, not an
  afterthought
- [**The moving parts**](the-moving-parts.md) —
  the library, the model list,
  capabilities, VFOs, the four tools,
  and the plain-text network protocol
- [**Installing Hamlib**](installing-hamlib.md) —
  one package on Debian and Ubuntu,
  the version check, and a first look
  at the model lists
- [**Your first connection**](your-first-connection.md) —
  a session with the dummy: reading
  its frequency, setting a new one,
  reading it back, and the dummy's
  honest character
- [**Frequency and mode**](frequency-and-mode.md) —
  hertz without makeup, the mode
  names, passbands, VFOs, and the
  numbered world of levels
- [**PTT and staying safe**](ptt-and-safety.md) —
  keying the dummy, the PTT type that
  must be named first, and the habits
  that keep software keying safe on
  real stations
- [**Rotators with rotctl**](rotators.md) —
  pointing an antenna by command, the
  prompting trap, and the dummy
  rotator that accepts orders it never
  carries out
- [**rigctld as a shared service**](rigctld-as-a-shared-service.md) —
  one rig opened once, many clients,
  shared state, the raw protocol, and
  the two warnings that belong to it
- [**Wiring up Fldigi and WSJT-X**](fldigi-and-wsjtx.md) —
  the Hamlib NET rigctl menu entry,
  the order of operations, and flrig,
  the neighbour with a face
- [**Permissions and serial devices**](permissions-and-serial.md) —
  device names, the dialout group,
  baud rates, and why device numbers
  move
- [**Debugging with verbose output**](debugging.md) —
  raising the lights with `-v`,
  reading a trace to the line that
  failed, and the error codes with
  names
- [**Recipes**](recipes.md) — read the
  frequency from a script, park the
  station, a digital-modes evening,
  prove a rotator answers, leave the
  station tidy
- [**The neighbours**](the-neighbours.md) —
  flrig, the makers' own software,
  programs with their own CAT, the
  library from code, and radios that
  live on the network
- [**Limits**](limits.md) — uneven
  backends, a dummy that is a polite
  fiction, a daemon with no passwords,
  and the legal edge, stated plainly
- [**Troubleshooting**](troubleshooting.md) —
  "Feature not available", the silent
  daemon, the rotator that reads back
  zero, values eaten by prompts, and
  the traps this guide's lab fell
  into so you don't have to
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Programming a specific radio's
memories channel by channel (the
makers' tools and CHIRP live there),
designing antennas or rotator
hardware, contest strategy, and the
audio side of digital modes — the
sound card setup belongs to the
programs and their own guides.
Building Hamlib from source is
pointed at, not taught; this guide
stays with the packaged tools a
station actually runs.

## How to use this guide

Read chapter 1 for the model and the
rule, and chapter 2 before connecting
anything — knowing which part holds
what turns later confusion into a
named problem. Chapters 3 and 4 take
you from the install to a live
session with the dummy in one
sitting; do not wire a real radio
until the dummy sessions feel boring.
Chapter 6 is not the optional chapter
either: understand PTT types and the
safety habits before any program is
allowed to key anything. The lab's
transcripts live in
`hamlib-lab/transcripts/`, and its
distilled commands and notes are
`examples/commands.txt` and
`examples/lab-notes.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Fldigi for Mere Mortals](https://paulsullivan33-dev.github.io/fldigi-for-mere-mortals/)
- [WSJT-X for Mere Mortals](https://paulsullivan33-dev.github.io/wsjtx-for-mere-mortals/)
- [Ham Radio for Mere Mortals](https://paulsullivan33-dev.github.io/ham-radio-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/hamlib-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/hamlib-for-mere-mortals/blob/main/LICENSE-CODE).
