# Rotators with rotctl

This chapter ends with you commanding
an antenna rotator — asking where it
points, ordering it elsewhere — on the
dummy rotator, and with one honest
disappointment understood and
explained. Everything said about rigs
has a twin here: rotators are the
other half of a station that software
can move.

## The parallel tool

rotctl mirrors rigctl flag for flag:
`-m` picks the model, from its own
list:

```bash
rotctl --list
```

The lab's list ran to about fifty
rotator models — the big amateur
makers, a few satellite trackers, and
at model 1 the **dummy rotator**, the
lab's instrument for this chapter. A
rotator's capabilities dump is shorter
than a rig's:

```bash
rotctl -m 1 --dump-caps
```

The facts that matter: the type is
**Az-El** — it turns in **azimuth**
(compass bearing, 0–360°) and tilts in
**elevation** (0° at the horizon, 90°
straight up). Azimuth-only rotators —
most HF beams — report just the first.
The dummy's ranges are generous:
azimuth −180° to 450° (it allows for
the overlap real rotators build in so
the cable does not bind), elevation
0–90°.

## Pointing it

At the `Rotator command:` prompt, `p`
asks where the antenna points and
`\set_pos` (or `P`) orders a new
position, azimuth first, then
elevation:

```text
Rotator command:        (type: \get_pos)
Azimuth: 0.00
Elevation: 0.00

Rotator command:        (type: \set_pos 135 30)
```

Two traps, both found by the lab and
now marked for you. First, the
one-letter `P` in interactive mode
does not take its numbers on the same
line the way rigctl's `F` does — it
answers with its own prompts,
`Azimuth: Elevation:`, and reads each
value on its own line. Scripts that
assume the rigctl shape will feed it
nonsense. The long command
`\set_pos 135 30` takes both on one
line and is the form worth learning.

Second — the honest disappointment:
ask the dummy for its position after
setting one, and it still says 0.00,
0.00. The verbose output proves the
command truly arrived:

```text
rot_set_position called az=135.00 el=30.00
dummy_rot_set_position called: 135.00 30.00
rot_get_position: got az=0.00, el=0.00
```

The dummy rotator accepts orders,
cheerfully, and does not simulate a
turning antenna; its position is a
fixed zero. Contrast the dummy *rig*,
which remembers everything. The lesson
generalises, and it is one of the most
valuable in this guide: **a command
accepted is not a motion performed.**
On a real rotator the same command
pair — set, then get — is how you
verify the antenna actually went, and
the pair matters because cables slip,
rotators stall, and software that
assumes is software that points a beam
at the ground all weekend.

## Movement, parking, stopping

The fuller vocabulary, for real
hardware: `\move` starts continuous
motion in a direction at a speed (and
is how satellite trackers track, in
many small moves), `\stop` halts it,
`\park` sends the antenna to its rest
position, and `\reset` clears a
controller's fault state. The dummy
accepts all of them without effect
beyond the same fixed read-back, so
this guide demonstrates `\set_pos` and
`\get_pos` end to end and describes
the rest from the documentation —
labelled as such in `criteria.md`,
not dressed up as runs.

## rotctld

Everything chapter 8 does for rigs
exists for rotators: **rotctld** opens
one rotator and serves it on a network
port (4533 by default), so a satellite
tracker and a logging program can
share the turning gear. The protocol
is the same text protocol, the client
is `rotctl -m 2` — the rotator list
has its own network backend — and the
lab verified the pattern against
rigctld rather than twice, again
recorded plainly in `criteria.md`.

Check: you can list rotator models,
read a capabilities dump, command a
position in the one-line form, and
explain — with the trace to prove it —
why the dummy's read-back stays at
zero. That explanation is worth more
than the commands: it is the
difference between operating a station
and operating a fiction about one.
