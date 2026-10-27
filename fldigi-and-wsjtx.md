# Wiring Up Fldigi and WSJT-X

This chapter ends with the two
digital-mode programs most likely to
bring a reader to Hamlib — WSJT-X and
Fldigi — connected the way chapters
1–8 have prepared: not to the radio,
but to the daemon that owns the radio.
Their screens belong to their own
guides in this series; here they
appear as what they are to Hamlib,
two more clients of the protocol.

## The shape of the connection

Both programs offer a list of rigs at
their radio settings. That list is
Hamlib's model list — the same one
from `rigctl --list`, wearing a menu.
Every entry on it opens the radio
directly. The entry this chapter is
about is the odd one:

> **Hamlib NET rigctl**

Choosing it means "do not open a
radio; connect to a rigctld". Beside
it, both programs show a **network
server** field. That is where
`127.0.0.1:4532` goes — the daemon
from chapter 8, same machine, default
port. With that set, the program's
frequency display, mode setting and
PTT all flow through the daemon,
alongside every other client.

The order of operations matters and
is the whole recipe:

1. Start rigctld against the (dummy,
   for now) rig.
2. Prove it with a terminal client
   and with netcat, as chapter 8 did.
3. Only then point the program at it.

Step 2 is what turns the usual
twenty-minute settings safari into a
two-minute one: every layer below the
program is already known-good, so
anything still wrong is inside this
one program's settings page.

## WSJT-X

In WSJT-X: Settings, Radio tab. Set
**Rig** to *Hamlib NET rigctl*, and
**Network Server** to `127.0.0.1:4532`.
**PTT Method** to CAT — the daemon
keys the rig over the protocol, the
`-P RIG` idea from chapter 6 in GUI
form. Mode and split settings are
WSJT-X's own business (its guide in
this series covers them); press its
**Test CAT** button and it asks the
daemon for the frequency, and **Test
PTT** keys and unkeys — on the dummy,
harmlessly, which is why the dummy
daemon is the right first partner for
a program whose whole job is keying a
rig on a schedule.

What WSJT-X does with the connection,
once it trusts it: reads the frequency
before every transmission period,
complains audibly if the dial and the
program disagree ("rig control error"
is usually chapter 8's daemon having
stopped), and keys the rig for its
15-second transmissions. A station
running this arrangement can watch all
of it happen from a third client —
rigctl, model 2 — which is a fine
party trick and a better debugger.

## Fldigi

Fldigi's rig control lives under
Configure → Rig control, and it offers
several tabs. Two matter here:

- The **Hamlib** tab: pick *Hamlib NET
  rigctl* from the rig list (or check
  the **rigctld** option where the
  version offers it), give the address
  and port, and initialise. Fldigi
  then reads and sets frequency and
  mode through the daemon, and its
  PTT follows.
- The **flrig** tab: Fldigi's sibling
  program flrig — itself model 4 on
  Hamlib's list — is a rig-control
  panel with its own server, and
  Fldigi can drive the rig *through
  flrig* instead. That is a different
  architecture (a GUI owns the rig,
  programs ask it), closer to chapter
  13's neighbours than to rigctld,
  and a good choice when a human also
  wants a big friendly tuning knob on
  screen. Both arrangements are
  legitimate; mixing them — Fldigi
  pointing at flrig while WSJT-X
  points at rigctld pointing at the
  same serial port — recreates the war
  chapter 8 ended. One owner per rig.

## What this guide verified, honestly

The lab verified the whole Hamlib
side for real: the daemon, the
protocol, the shared state, PTT
through the port (transcripts t05,
t05b). The programs' settings screens
are described here from the projects'
documentation and the series' own
Fldigi and WSJT-X guides — the lab's
machine ran neither GUI, and
`criteria.md` says so in as many
words. The claim this chapter makes
is the checkable one: these programs
are clients of the same protocol you
have already driven by hand, so once
the daemon answers netcat, their
connection is a settings page, not a
mystery.

Check: with the dummy daemon running,
one of the two programs set to Hamlib
NET rigctl shows 145,000,000 (or
whatever a terminal client last set)
in its frequency window — and changing
it in a terminal changes it on the
program's display. Two programs, one
rig, no fighting.
