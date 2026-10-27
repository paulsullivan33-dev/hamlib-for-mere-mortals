# Troubleshooting

The failures this guide's lab met in
person come first; the field's
classics follow. Each entry names
the symptom, the cause the evidence
supported, and the check that tells
them apart.

## "Feature not available" on a healthy rig

**Symptom:** `t` (and `T`) fail on
the dummy with a wall of trace text
ending in *Feature not available*,
while frequency commands work.
**Cause:** no PTT type is set —
the dummy's is "None" — so Hamlib
refuses to guess how keying works
(chapter 6). **Check:** the
capabilities dump's "PTT type" line.
**Fix:** `-P RIG`, or the method
your station uses. On a real rig
the same message can also mean
exactly what it says; the dump is
how you tell which.

## The daemon answers nothing at all

**Symptom:** netcat to port 4532
connects, a command goes in, silence
comes out. **Cause:** the command
does not exist. `Z`, the lab's
deliberate nonsense, got *no reply*
— rigctld does not send errors for
unknown short commands
(transcript t05b). **Check:** send
a command that cannot be wrong,
`f`. If `f` answers, the daemon and
rig are fine and the failing
command's spelling or backslash is
the fault. Silence from `f` too
means the layer below: is rigctld
even running, on the port you think,
holding a rig that opens?

## The rotator reads back zero forever

**Symptom:** `\set_pos` accepted,
`\get_pos` unmoved. **Cause, on the
dummy:** the backend does not
simulate motion (chapter 7, with the
verbose proof). **Cause, on real
hardware:** the same pair with
numbers that stay zero is a stalled
rotator, a slipping mast clamp, or
the wrong device — which is why the
dummy's harmless version is worth
meeting first. **Check:** `-vvvvv`
shows whether the backend was even
called.

## Values eaten by prompts

**Symptom:** a piped rotctl script
sets nothing and reads nonsense.
**Cause:** the one-letter `P`
prompts for azimuth and elevation
*on their own lines*; a script that
sends `P 90 45` feeds it a script's
worth of confusion. **Fix:** use the
long form, `\set_pos 90 45`, in
scripts (chapter 7). rigctl has the
same family of surprises wherever a
command's arguments were designed
for typing; when a pipe misbehaves,
run the session by hand once and
watch the prompts.

## Permission denied on the serial device

**Symptom:** opening `/dev/ttyUSB0`
fails instantly. **Cause:** your
account is not in the device's group
(chapter 10). **Check:** `ls -l` on
the device, `id` for your groups.
**Fix:** join the group, log out,
log in — and do not be surprised
when it starts working only after
the re-login.

## Garbage, or silence, from a real rig

**Symptom:** the session opens but
answers are wrong, late, or absent.
**Cause, in order of likelihood:**
the wrong device name (the other
adapter, the other port of the same
rig), a baud rate the rig is not
set to (`-s`, chapter 10), or the
wrong model number — a cousin's
backend speaking a dialect that
almost fits. **Check:** `--dump-caps`
at `-vvvvv`: the trace shows the
bytes out and the bytes back, and a
protocol error (−9) at the first
command closes the argument.

## The port is already in use

**Symptom:** rigctld will not start,
or a client behaves as if two rigs
exist. **Cause:** a daemon from an
earlier session is still running —
started by a service, a script, or
you, in a terminal that has since
scrolled away. **Check:** ask the
port who it is with netcat
(`\dump_state` names the rig), and
`pgrep -a rigctld` for the process.
**Fix:** one daemon per rig, started
deliberately, chapter 8's rule.

## Everything worked until the program updated

**Symptom:** a client's rig list or
behaviour changed overnight.
**Cause:** Hamlib is a shared
library; a program update can bring
a new one, and model numbers and
backend grades occasionally move
between releases. **Check:**
`rigctl --version` and the model
list, against the program's About
box. The rigctl reproduction rule
from chapter 11 applies with full
force: settle it at the terminal
first, then return to the program
with the answer already known.
