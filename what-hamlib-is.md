# What Hamlib Is

A modern amateur radio — a **rig** — is
not only knobs and a dial. Nearly every
rig made in the last thirty years has a
serial or USB port around the back, and
through that port a computer can do
almost everything the front panel can:
read the frequency, change the frequency,
set the mode, and press the
**push-to-talk** switch — **PTT** — that
puts the rig on the air. Talking to a
rig this way is called **CAT**, computer
aided transceiver control, and every
manufacturer invented a different dialect
of it. Different commands, different
speeds, different quirks, one per maker
and often one per model.

**Hamlib is the library that learned all
of those dialects so your programs do not
have to.** It is a free library — the
name is short for *ham library* — with a
**backend** inside it for each supported
radio: your program asks Hamlib to "set
the frequency to 14,074,000 Hz", in one
standard way, and the backend for your
radio translates that into whatever bytes
your particular rig expects. Logbook
programs, digital-mode programs like
Fldigi and WSJT-X, satellite trackers,
contest loggers — hundreds of programs
speak to radios through Hamlib, which is
why one small library sits quietly
underneath a large part of the hobby.

This guide teaches Hamlib from the
outside, through the four tools that
come with it: **rigctl**, which lets you
drive a radio from a terminal; **rotctl**,
the same idea for antenna rotators;
**rigctld**, which turns one radio into a
network service that many programs can
share; and **rotctld**, its rotator twin.
You will learn all four on a radio that
does not exist — Hamlib's **dummy rig**,
a backend that pretends to be a radio and
answers every command from memory — so
every procedure in this guide is safe to
try, in any order, at any hour.

## The rule, stated before everything else

Most software in this series cannot hurt
anyone if you get it wrong. This one can
cause real-world trouble, because the
same command that tunes a receiver can
also key a transmitter. So this guide has
one rule, and it is not fine print:

> **The software can key a real
> transmitter. Stay inside your licence,
> your band edges, and dummy loads while
> you learn.**

Staying inside your licence means
transmitting only on frequencies and
modes your licence allows, with your
callsign given the way the rules require
— software pressing PTT does not change
one word of that. Band edges means the
dial frequency is not the whole signal:
a transmission has width, and software
that retunes a rig mid-session can park
that width somewhere you never looked.
A **dummy load** is a resistor in a can
that soaks up radio energy instead of
radiating it — the practice range of
radio. While you learn any of this, the
order of operations is: dummy rig in
software first, then a real rig receiving
only, and transmitting last of all, into
a dummy load, at low power, on a
frequency you are licensed for. The lab
in this guide never left the first of
those steps, and everything in it can be
learned there.

## Why anyone would do this

- **Digital modes.** Programs like
  WSJT-X and Fldigi need to know the rig's
  frequency and need to key it for each
  transmission. They could each speak
  every dialect; instead they ask Hamlib.
- **Logging and chasing.** A logbook that
  reads the rig never contains a mistyped
  frequency. Click a station in a DX
  cluster and the rig jumps to it.
- **Remote and headless operation.** A
  rig in another room — or another
  building — can be driven through
  rigctld by programs elsewhere on the
  network (chapter 8), and a rotator can
  follow a satellite pass on its own.
- **Scripting.** Anything rigctl can do
  from a terminal, a script can do at
  3 a.m.: retune to a beacon, record the
  band, park the antenna.

## A familiar shape

Hamlib is one library with many backends,
and the tools are thin shells over it.
You pick a radio by **model number** —
an integer from a list the tools print
themselves (`rigctl --list`, chapter 2).
Model 1 is the dummy. Model 2 is a
special backend that talks to a rigctld
server instead of a radio, which is how
the sharing in chapter 8 works. Every
real radio has its own number, in blocks
per manufacturer.

A first taste, after installing
(chapter 3):

```bash
printf 'f\nq\n' | rigctl -m 1
```

That asks model 1 — the dummy — for its
frequency, and quits. The dummy wakes up
on 145,000,000 Hz, so it answers:

```text
Frequency: 145000000
```

No radio, no cable, no licence required,
and nothing transmitted: the perfect
first connection, and chapter 4 takes it
properly, one command at a time.

## Who it is for

The operator whose logging or digital
program says "rig control not working"
and who wants to test the radio link
without the program in the way. The
curious owner of a modern rig wondering
what the USB port is for. The script
writer who wants the antenna pointed
before the satellite rises. The common
thread is one person, their own station,
and a wish to drive it with software
instead of only by hand.

## What it is not

Hamlib is not a radio program in itself:
it has no waterfall, no decoder, no
logbook — it is the plumbing those
programs stand on. It does not decode
anything; it only controls hardware. It
is not a substitute for knowing your
licence conditions — it will cheerfully
tune a rig outside the amateur bands if
the rig allows it, because receive-only
tuning is legitimate and common, and the
responsibility for transmitting stays
with the operator, exactly where the
regulations put it. And it is not
uniform: three hundred backends written
over twenty-five years vary in polish,
and chapter 14 is honest about that.
