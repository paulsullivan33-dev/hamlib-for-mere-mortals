# Your First Connection

This chapter ends with you in a live
session with a radio: asking its
frequency, changing it, and hearing the
change read back. The radio is the
**dummy**, model 1 — a backend that
pretends, perfectly behaved and
impossible to damage — so the session
can be repeated until it is boring.
Nothing here can transmit, and no
licence is needed. This is the practice
range; move in.

## Starting the session

rigctl takes the model number with
`-m` and then waits for commands:

```bash
rigctl -m 1
```

It prints a blank line and a prompt,
`Rig command:`. Each command is one
letter, sometimes with a value after
it, ended with Enter. `q` quits. Three
commands make a first conversation —
the lab's transcript
(`hamlib-lab/transcripts/t03-first-session.txt`)
shows the whole exchange:

```text
Rig command:            (type: f)
Frequency: 145000000

Rig command:            (type: F 14074000)

Rig command:            (type: f)
Frequency: 14074000
```

Reading it as a story: `f` asks for the
frequency, and the dummy wakes up, as
it always does, on **145000000** — 145
MHz, in the two-metre amateur band. `F`
with a number sets the frequency, in
**hertz, always in hertz**: 14074000 is
14.074 MHz, in the twenty-metre band —
the frequency where WSJT-X's FT8 mode
lives, chosen here precisely because
nothing can be transmitted. Setting
commands answer with silence, which is
Hamlib's way of saying success (a
failure prints an error instead, chapter
15 shows one). The second `f` reads
back the new frequency: proof the
backend remembered it, the way a real
rig's dial would now read 14.074.

## Asking in a pipe

Interactive typing is for learning;
scripts feed rigctl from a pipe, one
command per line:

```bash
printf 'f\nF 14074000\nf\nq\n' | rigctl -m 1
```

Same conversation, no typing, and the
pattern every later chapter uses when
it wants a transcript you can re-read.
That is the whole shape of Hamlib in
miniature: an operation, an answer,
state that persists between them.

## The dummy's character

Meet the instrument honestly, because
the rest of the guide leans on it:

- It **remembers**: frequency, mode,
  PTT, its levels — within a session,
  and, through rigctld, across clients
  (chapter 8). It forgets everything
  when the session ends.
- It is **generous**: its capabilities
  dump claims almost every function
  Hamlib knows, across 150 kHz to
  1.5 GHz. Real radios are more limited
  and more specific.
- It **invents meter readings**. Ask
  for the signal strength (`l STRENGTH`)
  twice and the numbers differ — the lab
  saw −37 and −30 on consecutive calls.
  There is no signal. The dummy is
  acting one.
- It starts every session on 145 MHz in
  FM, whatever happened last time.

## What changes with a real radio

One flag. A real rig adds `-r` for its
device: on Linux, typically
`-r /dev/ttyUSB0` for a USB serial
cable, with the model number of your
actual radio in place of 1. Everything
else in the session is identical —
which is exactly the point of the
library, and why practising on the
dummy is not a toy version of the real
thing. Chapter 10 prepares that step:
which device name, whose permission,
and why real radios also want a baud
rate (`-s`) the dummy never asks for.

Check: you have connected, read a
frequency, set one, and read it back.
Chapter 5 stays in this session and
goes wider: modes, passbands, VFOs and
levels.
