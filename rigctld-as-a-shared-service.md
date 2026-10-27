# rigctld as a Shared Service

This chapter ends with one rig —
still the dummy — opened once by a
daemon, and two separate clients
driving it at the same time, watching
each other's changes. This is the
arrangement real stations use, and it
is the bridge to Fldigi and WSJT-X in
chapter 9.

## Why a daemon at all

A serial port opens for one program
at a time. But a working station wants
the rig shared: the digital program
needs frequency and PTT, the logbook
wants the frequency for each entry,
the operator wants a terminal handy
for poking at it. Without help, the
programs fight over the port and the
usual peace treaty is "close
everything else first".

**rigctld** is the peace treaty. It
opens the rig once, as its only
client, and serves Hamlib's text
protocol (chapter 2) on a TCP port —
4532 by default. Any number of
programs then connect as clients.
Their commands are queued to the one
radio, and its state — one frequency,
one mode, one PTT — is shared truth
for all of them.

## Starting it

The daemon takes the same flags as
rigctl, plus its port:

```bash
rigctld -m 1 -P RIG -t 4532
```

Read it left to right: serve model 1
(the dummy), keyed via the rig
protocol, on TCP port 4532. It runs
in the foreground, quiet, until
stopped with Ctrl-C. The flags worth
knowing beside `-t`: `-m` the model,
`-r` the serial device for a real
rig, `-s` its baud rate, `-T` the
address to listen on, and `-P` the
PTT type, passed through to the rig
it opens — the lab's PTT chapter
discovery applies to the daemon
exactly as to rigctl.

## Clients: model 2 is the door

A client program does not open the
rig; it opens the daemon, by choosing
the **NET rigctl** backend — model 2 —
and giving the daemon's address as
its "device":

```bash
printf 'f\nq\n' | rigctl -m 2 -r 127.0.0.1:4532
```

From the lab's transcript (t05),
proof that the state is truly shared.
Client A connects and moves the rig:

```text
(type: f)   Frequency: 145000000
(type: F 7074000)
```

Client B — a second, separate rigctl,
started after A quit — connects and
asks:

```text
(type: f)   Frequency: 7074000
```

B sees A's frequency, because there
was only ever one rig, wearing two
conversations. A third client keyed
PTT through the daemon and read it
back (`T 1`, `t` → `PTT: 1`) — the
daemon passes everything, which
chapter 6's safety discussion already
warned about and the next section
underlines.

## Speaking to it raw

The protocol needs no Hamlib client
at all. With netcat (chapter 2 showed
the shape; transcript t05b has the
lab's session):

```bash
printf 'f\n' | nc -q 1 127.0.0.1 4532
printf '\\dump_state\n' | nc -q 1 127.0.0.1 4532
```

This is more than a parlour trick. It
is the fastest health check a shared
rig has: if the raw answer comes
back, the daemon and the rig are
alive and any misbehaving program's
problem is its own. It is also how
small scripts and home-grown tools —
a band-change button on a web page,
a logger in a language with no
Hamlib binding — join the station.

## Running it as a service

A daemon you start by hand dies with
your terminal. Real stations start
rigctld at login: on Linux, a systemd
user service whose whole content is
the command above (the project's
documentation and your distribution's
systemd guide cover the unit file;
this guide's lab started the daemon
by hand so its transcripts show the
bare command, and `criteria.md`
labels the service form
documented-only). Whichever way it
starts, start it **before** the
clients, and teach the clients the
address once.

## The two warnings that belong here

**No authentication.** Anyone who can
reach the port can drive the rig —
including `T 1`. The default is safe
only because the daemon listens
where you tell it: keep it on
127.0.0.1 (this machine) unless the
network is yours end to end, and
treat a rigctld port forwarded to
the internet the way you would treat
a microphone left open in the shack,
except the microphone can transmit.

**No arbitration.** The daemon queues
commands; it does not referee them.
If the logbook sets 7,074,000 and the
operator's terminal sets 14,074,000,
the rig goes to whichever arrived
last, and both clients are "right".
Shared rigs run on manners: one
program drives, others read, or the
operators agree who is driving
tonight.

Check: one daemon, two clients, one
frequency seen by both, and a raw
netcat answer from the port. Chapter
9 replaces client B with the two
programs most readers actually run.
