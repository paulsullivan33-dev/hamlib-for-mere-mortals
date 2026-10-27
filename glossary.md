# Glossary

The terms, collected in one place.
Each is defined where it first
appears in the guide as well; this
is the quick-reference copy.

**Azimuth** — the compass bearing an
antenna points at: 0° north, 90°
east, 180° south, 270° west.

**Backend** — the part of Hamlib
that knows one family's radios and
translates standard requests into
that family's commands.

**Baud rate** — the speed of a
serial link, in symbols per second.
Rig and computer must be set to the
same rate or the conversation is
garbage.

**Capabilities** — the list of what
a backend says its radio can do:
ranges, modes, functions, levels.
Printed by `--dump-caps`.

**CAT** — computer aided transceiver
control: running a radio from a
computer through its serial or USB
port. Also the name of the command
dialects themselves.

**Daemon** — a program that runs in
the background, waiting to serve.
rigctld and rotctld are daemons.

**Dialout** — the traditional Unix
group that owns serial devices;
membership is the usual fix for
"Permission denied" on a rig's port.

**Dummy load** — a resistor that
absorbs a transmitter's output as
heat instead of radiating it. The
safe way to test anything that keys
a rig.

**Dummy rig** — Hamlib's model 1: a
backend that pretends to be a radio,
remembering state in memory. The
practice instrument of this guide.

**Elevation** — how far an antenna
tilts up from the horizon: 0° flat,
90° straight overhead.

**Level** — a numbered rig setting
read with `l` and set with `L`:
gain, squelch, power, meter
readings.

**Mode** — the shape of
demodulation a rig applies: USB,
LSB, CW, FM, AM, and the data
variants. Travels with a passband.

**Model number** — the integer that
names a backend: 1 the dummy, 2 the
network backend, the rest in makers'
blocks. Printed by `--list`.

**NET rigctl** — model 2: the backend
that connects to a rigctld server
instead of a radio, so any Hamlib
program can be a client.

**Passband** — the width, in hertz,
of the signal a rig lets through its
filters in a given mode.

**PTT** — push-to-talk: the switch,
physical or software, that puts a
rig into transmit. `T` sets it, `t`
reads it, and chapter 1's rule
governs it.

**PTT type** — *how* keying is done:
through the rig's commands (RIG), a
serial line (RTS, DTR), a sound
interface (CM108), a GPIO pin — set
with `-P`.

**Rotator** — the motor that turns
(and sometimes tilts) an antenna;
driven by rotctl, served by rotctld.

**RPRT** — the return-code prefix on
rigctld's wire protocol: `RPRT 0` is
success, a negative number is an
error with a name.

**Split** — receiving on one VFO and
transmitting on another, the
arrangement satellite and DX work
uses.

**VFO** — variable-frequency
oscillator: a rig's tuning dial in
electronic form. Many rigs have two,
A and B, and most commands act on
the current one.
