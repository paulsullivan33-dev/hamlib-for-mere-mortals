# The Moving Parts

Hamlib has more pieces than its four
commands suggest. Knowing which piece
holds what is the difference between
"Hamlib is broken" and "the daemon is
fine; this client asked for a feature
the radio does not have" — a real
distinction, as this guide's own lab
learned (chapter 15).

## The library and the backends

At the centre is the library itself,
`libhamlib`. It offers programs one
standard set of operations — get and set
frequency, get and set mode, key and
unkey PTT, read the meter, and about a
hundred more — and it routes each
operation to a **backend**: the chunk of
code that knows one family's radios.

Each backend covers one **model** (or a
family that shares a dialect), and each
model has a number. Ask the tool for the
list:

```bash
rigctl --list
```

The lab's copy printed 284 lines: the
dummy at 1, the network backend at 2,
FLRig at 4, a second dummy at 6, and
then the manufacturers in numbered
blocks — Yaesu from 1001, Icom from 2001,
Kenwood from 3001, and so on. Each line
also grades the backend: **Stable**,
Beta, Alpha, or Untested. That grade is
part of the truth about this tool, and
chapter 14 returns to it.

## Capabilities: what this radio can do

Two radios rarely offer the same
controls, so before using one, Hamlib
asks the backend what it can do. The
answer is the **capabilities** dump:

```bash
rigctl -m 1 --dump-caps
```

For the dummy it runs to 221 lines:
the frequency ranges it covers, the
modes it knows, which functions can be
read and which can be set, which levels
exist (preamp, attenuator, RF gain,
squelch, and dozens more), even the
CTCSS tone list. Two entries matter more
than the rest while you learn:

- **Can set PTT / Can get PTT** — whether
  this backend will key the transmitter
  at all (chapter 6).
- The **RX and TX ranges** — where the
  rig can listen, and the smaller set of
  places it can transmit.

Real backends describe real limits:
a handheld may report two bands and no
preamp; a contest rig, nine bands and
everything. Programs grey out buttons
based on this dump, which is why a
button can be missing in Fldigi for one
radio and present for another with
nothing "wrong" anywhere.

## VFOs

Most rigs have at least one **VFO** —
variable-frequency oscillator, the
tuning dial's electronic form — and
serious rigs have two, VFO A and VFO B,
so you can listen on one frequency and
be ready on another. Hamlib names them
(`VFOA`, `VFOB`, sometimes `Main` and
`Sub`), and most rigctl commands act on
the *current* VFO. The command `v` asks
which one that is; the dummy answers
`VFOA`. Satellite operators care most
about this: their uplink and downlink
live on two VFOs at once, in **split**
operation.

## The four tools

- **rigctl** — an interactive terminal
  shell over one rig. Type one-letter
  commands (`f` for frequency), read the
  answers, `q` to quit. It also reads
  commands from a pipe, which turns it
  into a scripting tool. Most of this
  guide lives here.
- **rigctld** — a daemon (a program that
  runs in the background) that opens one
  rig and serves it on a TCP network
  port, 4532 by default. Many programs
  can then share the rig at once,
  because the daemon holds the serial
  port they cannot all open (chapter 8).
- **rotctl** — rigctl's twin for antenna
  rotators: position, movement, parking.
- **rotctld** — the rotator daemon, on
  port 4533 by default.

The package also carries a few
specialists the guide mentions only in
passing: `ampctl` for amplifiers,
`rigmem` for a rig's memories, and
measurement tools like `rigswr`.

## The network protocol

rigctld speaks a plain-text protocol —
unusual, and a gift for learning. A
client connects to the port and sends
one command per line. Short commands
mirror rigctl's single letters; long
commands start with a backslash and say
their names. From the lab, over a plain
TCP connection:

```text
f            →  145000000
\get_freq    →  145000000
F 14060000   →  RPRT 0
```

Answers to questions are the bare value.
Answers to commands that set something
are a **return code**: `RPRT 0` means
success, a negative number names the
error. One command deserves its own
mention: `\dump_state` prints what the
daemon's radio can do — ranges, modes,
filter widths — in a compact numeric
form, and it is the first thing client
programs ask for when they connect.
That is how WSJT-X knows which buttons
to offer you.

One more property of the protocol, also
from the lab: a command that does not
exist gets **silence**, not an error.
Chapter 15 makes a troubleshooting rule
of it — when a client gets nothing back,
suspect the command before the network.

## Which part holds what

When something misbehaves, this map
usually names the culprit:

- Wrong frequency shown by every
  program: the rig, or the backend.
- One program wrong, others right: that
  program's settings.
- Everything suddenly disconnected: the
  daemon, or the serial device under it.
- A command rejected with "feature not
  available": the capability really is
  absent — or the PTT type is unset,
  which chapter 6 explains.
