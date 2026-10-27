# PTT and Staying Safe

This chapter ends with you keying a
transmitter that does not exist — on,
off, read back — and understanding why
the command that does it deserves more
respect than any other in this guide.
PTT is the point where software stops
moving numbers and starts moving
energy: **push-to-talk** is the switch
that puts a rig into transmit.

## The two commands

At the rigctl prompt:

- `t` — ask: is the rig transmitting?
  The answer is `PTT: 0` (receiving) or
  `PTT: 1` (transmitting).
- `T 1` — transmit. `T 0` — stop.

On the dummy, from the lab's
transcript (t04):

```text
Rig command:            (type: t)
PTT: 0
Rig command:            (type: T 1)
Rig command:            (type: t)
PTT: 1
Rig command:            (type: T 0)
Rig command:            (type: t)
PTT: 0
```

## The catch the lab found for you

Run the same session against the bare
dummy — `rigctl -m 1`, no extra flags —
and `t` fails:

```text
Feature not available
```

followed by a dump of internal trace
lines (chapter 11 reads one properly).
The capabilities dump explains it: the
dummy's **PTT type** is "None". A PTT
type tells Hamlib *how* the keying is
done — through the rig's own command
set ("RIG", the CAT way), by wiggling a
serial line (RTS or DTR) on a separate
port, through a sound-card interface
box (CM108), or through a GPIO pin.
Until you name one, rigctl declines to
guess, and get_ptt has nothing to
report. The fix is one flag:

```bash
rigctl -m 1 -P RIG
```

`-P RIG` says: key it through the rig
protocol — the right answer for the
dummy, and for most modern CAT rigs
too. (Its sibling backend, model 6,
the "Dummy No VFO", declares itself
"Rig capable" and needs no flag; the
inconsistency is real Hamlib life, and
chapter 14 owns it.)

On a real rig the same idea applies
with higher stakes: choose the PTT
method your station actually uses, and
if it is a serial line on a separate
port, `-p` names that port. A wrong
choice here is not a software bug; it
is a transmitter keyed by accident or
not at all.

## Why this chapter is next to chapter 1's rule

Everything chapter 1 said returns with
knobs on:

- **A program can key your rig while
  you are out of the room.** WSJT-X
  does it by design, every other
  minute, for hours. That is normal,
  licensed, and exactly why the
  frequency it is parked on matters.
- **Check the frequency before you
  enable transmit in any program** —
  with `f`, on the dummy first, then on
  the real rig receiving. The last look
  at the dial is a habit software makes
  easy to skip and expensive to learn.
- **First transmissions go into a
  dummy load at low power**, with the
  rig's own power meter as the witness.
  A dummy load turns your test into
  heat, not radiation.
- **Every program that can key the rig
  is a program that can key the rig.**
  Through rigctld (chapter 8), *any*
  client on the port can send `T 1` —
  including you, in a terminal, by
  typo. Know which clients are
  connected before the antenna is.

## Timeouts and watchdogs

Software keying fails in one memorable
way: a program crashes mid-transmission
and leaves the rig keyed. Defences
exist at several layers, and a careful
station uses at least two: the rig's
own transmit time-out timer (a menu
setting on most modern rigs), Hamlib
clients that unkey on exit, and the
oldest one — an operator within reach
of the power switch. Set the rig's
timer before your first automated
transmission, not after your first
stuck one.

Check: on the dummy with `-P RIG`,
`T 1` then `t` reads 1, and `T 0`
returns calm. You have keyed the only
transmitter in this guide that cannot
hurt anyone, and you know the flag,
the type menu, and the rules that go
with it.
