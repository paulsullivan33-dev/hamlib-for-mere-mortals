# Frequency and Mode

This chapter ends with you driving the
two controls every other feature stands
on: where the rig is tuned, and how it
listens. It uses the dummy throughout,
so the bands are safe to wander.

## Frequency: f and F

The pair from chapter 4 generalises:

- `f` — get the frequency, in hertz.
- `F <hz>` — set it, in hertz.

Hertz, always, with no separators:
14.074 MHz is `14074000`, 145.5 MHz is
`145500000`. The value a program shows
with friendly dots and commas is the
same number wearing makeup. Scripts
love the plain form: a frequency read
by `f` can be stored, compared, and fed
back to `F` unchanged.

The long names work too, over the wire
and at the prompt: `\get_freq` and
`\set_freq`. The two dialects — one
letter for typing, a name for scripting
— run through the whole command set.

## Mode: m and M

A rig does not only sit on a frequency;
it listens in a **mode** — the shape of
demodulation it applies. `m` asks,
`M` sets, and a mode always travels
with its **passband**: the width of
signal, in hertz, that the rig lets
through its filters.

From the lab's first session: the
dummy wakes in FM with a wide passband,
and one command pair changes both:

```text
Rig command:            (type: m)
Mode: FM
Passband: 15000

Rig command:            (type: M USB 2700)

Rig command:            (type: m)
Mode: USB
Passband: 2700
```

The modes you will meet, in the names
Hamlib uses across all makers:

- **USB / LSB** — single sideband,
  upper and lower: voice and data on
  the HF bands, USB above 10 MHz, LSB
  below, by long convention.
- **CW** — Morse, with a narrow
  passband (500 Hz is typical).
- **FM / AM** — the broadcast-shaped
  modes: FM for VHF repeaters, AM for
  the broadcast and aviation bands.
- **PKTUSB / PKTLSB** (and kin) —
  "packet" sideband: the data setting,
  which on many rigs routes audio to
  the data socket and fixes the
  filters wide. Digital programs set
  this for themselves.
- **RTTY**, **CWR**, and radio-specific
  extras — the capabilities dump lists
  what a given backend offers.

Setting a mode without a passband uses
the backend's default width for that
mode. Setting both, as above, is the
explicit form, and `0` for the width
asks for the default too.

## Which dial: VFOs

`v` names the current VFO; `V` changes
it, where the rig has more than one:

```text
Rig command:            (type: v)
VFO: VFOA
```

**Split** — receiving on one VFO and
transmitting on the other — is the
arrangement satellite and DX work is
built on. Hamlib exposes it (`s` asks,
`S` sets, with the transmit VFO and its
frequency), and the dummy supports it
on paper. This guide demonstrates only
the single-VFO case end to end; split
on real hardware is chapter 14's
"uneven backends" warning at its most
practical, because two VFOs are where
makers' dialects diverge widest.

## Levels: the knobs with numbers

Beyond frequency and mode, a rig is a
wall of numbered settings — **levels**:
RF gain, squelch, audio volume,
preamp, power. `l` reads one, `L` sets
one, by name:

```text
Rig command:            (type: l STRENGTH)
Level Value: -30
```

`STRENGTH` is the received signal
strength, the S-meter, in decibels
relative to S9 on most backends. On
the dummy it is fiction that changes
between calls (chapter 4 confessed
this); on a real rig it is how a
program draws its meter. The
capabilities dump's "Get level" and
"Set level" lines are the menu of
names a backend offers, and they
differ — sometimes startlingly —
between radios. A cousin set, the
**functions** (`u` reads, `U` sets),
covers the on/off switches: noise
blanker, compressor, VOX, lock.

## A discipline worth adopting

End every experiment the way the lab
did: set something, read it back, and
only then believe it. Get-what-you-set
is not pedantry in a system where a
silent success and a silent rejection
look alike from a script. The read-back
is the confirmation the front panel's
display gives a human for free.

Check: on the dummy, set 7,074,000 Hz,
USB, 2,700 Hz passband; read both back;
ask which VFO you are on. All five
answers are in this chapter's
transcripts (t03, t03b).
