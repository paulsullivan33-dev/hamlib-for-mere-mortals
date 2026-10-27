# Recipes

Short, complete jobs, built only from
operations the earlier chapters ran
for real. Each names its chapter, so
a step that puzzles you has a home.
The rig in every recipe is the dummy
or the dummy daemon; substitute your
model, device and address where
chapters 4 and 10 taught them.

## Read the frequency from a script (chapters 4, 8)

The one-liner every home-grown tool
starts as — against a running daemon:

```bash
printf 'f\nq\n' | rigctl -m 2 -r 127.0.0.1:4532
```

or without any Hamlib client at all:

```bash
printf 'f\n' | nc -q 1 127.0.0.1 4532
```

The answer is the bare number.
Capture it in a shell variable and a
script can log it, compare it, or
sound an alarm when the dial is not
where the schedule says it should be.

## Park the station on a favourite (chapter 5)

Frequency, mode and passband in one
pipe, each set read back before the
script trusts it:

```bash
printf 'F 14074000\nM USB 2700\nf\nm\nq\n' | rigctl -m 1 -P RIG
```

The `f` and `m` read-backs are the
recipe's conscience — chapter 5's
discipline, automated. Point it at
the daemon (model 2) and every
connected program watches the rig
arrive where the script sent it.

## A digital-modes evening (chapters 6, 8, 9)

Start the shared rig, prove it, then
open the programs:

```bash
rigctld -m 1 -P RIG -t 4532        # terminal 1: leave running
printf 'f\n' | nc -q 1 127.0.0.1 4532   # terminal 2: proof
```

Then WSJT-X and Fldigi, both set to
*Hamlib NET rigctl* at
`127.0.0.1:4532`. Before enabling
transmit anywhere, chapter 6's
liturgy: check the frequency with
`f`, start low and into a dummy load,
and confirm the rig's transmit
time-out timer is set.

## Prove a rotator answers (chapter 7)

```bash
printf '\\get_pos\nq\n' | rotctl -m 1
```

A live answer — even the dummy's
eternal zero — proves the chain:
model, device, permission. On real
hardware, follow it with a small
`\set_pos` to somewhere harmless and
the same `\get_pos`, and watch the
numbers *move* (the dummy's do not;
that difference is chapter 7's
lesson, not a fault in your station).

## Ask a rig what it is (chapter 2)

For a newly arrived radio of unknown
pedigree, the capabilities dump is
the biography:

```bash
rigctl -m <model> -r /dev/ttyUSB0 --dump-caps | less
```

Compare its TX ranges with your
licence before anything else in this
book happens to that radio. If the
model number is unknown, the list
(`rigctl --list`) is searchable by
maker, and the dump's success is
itself the proof you picked right —
a wrong model rarely survives its
own dump.

## Leave the station tidy (chapters 6, 8)

The closing recipe, worth making a
habit: unkey if keyed, park the
rotator, stop the clients, stop the
daemon.

```bash
printf 't\nq\n' | rigctl -m 2 -r 127.0.0.1:4532   # PTT reads 0
```

A shared rig's last state is the
next session's first surprise; the
thirty-second check is cheaper.
