# Permissions and Serial Devices

This chapter ends with you able to
name the three things that stop a
real radio from answering — the wrong
device name, the wrong permission, the
wrong speed — and to check each in
seconds. The lab's machine had no
serial hardware at all, which it
recorded honestly (transcript t90);
the checks below are written so each
one states what a healthy answer
looks like on a real station, and
`criteria.md` labels this chapter
documented-only where it must.

## Where radios live: /dev

On Linux a serial device is a file
under `/dev`, and its name tells you
how it arrived:

- `/dev/ttyUSB0` — a USB-to-serial
  adapter, or a rig whose USB socket
  presents as one. The number climbs
  with each device plugged in.
- `/dev/ttyACM0` — a rig or interface
  speaking the USB modem protocol,
  common on newer radios.
- `/dev/ttyS0` — a true motherboard
  serial port, mostly history now.
- On macOS the same devices appear as
  `/dev/cu.usbserial-*` or similar; on
  Windows, as COM ports (COM3, COM5),
  which is what `-r` takes there.

Find yours by watching what appears
when the cable goes in:

```bash
ls /dev/ttyUSB* /dev/ttyACM*
```

plugged and unplugged, and compared.
Radio software asks for this name
constantly: it is rigctl's `-r`, and
rigctld's too.

## The permission that bites everyone

Serial devices belong to root and to
one group, traditionally **dialout**:

```text
$ getent group dialout
dialout:x:20:
```

If your account is not in that group,
opening the device fails with
*Permission denied* — and programs
report it in their own dialect
("cannot open port", "rig failure"),
rarely naming the cause. The checks:

```bash
id            # your groups, printed
ls -l /dev/ttyUSB0
```

The fix, once, on Debian and Ubuntu:

```bash
sudo usermod -a -G dialout $USER
```

then **log out and back in** — group
membership is granted at login, which
is why the fix so often "doesn't
work" for exactly one more session
and then mysteriously does. Some
distributions name the group `uucp`
or `serial` instead; the `ls -l`
output names the group that owns your
device, and that is the one to join.

## The speed that must match

A serial link runs at a **baud rate**
— symbols per second — and rig and
computer must agree on it. Hamlib
backends know each model's factory
default and use it unless told
otherwise with `-s`:

```bash
rigctl -m 1020 -r /dev/ttyUSB0 -s 38400
```

(model 1020 is a real radio on the
list from chapter 2 — an FT-817; the
command shape is the chapter's whole
point.) A mismatched speed produces
the cruellest symptom in this field:
silence, or garbage, with the cable
demonstrably fine. When a new rig
will not talk, the order of suspicion
is device name, permission, speed —
in that order, because that is the
order of how often each is wrong.

## Two modern wrinkles

**Numbers move.** ttyUSB0 today can
be ttyUSB1 tomorrow if another
adapter was plugged in first.
Stations with several devices tame
this with the stable names under
`/dev/serial/by-id/`, which encode
the device's maker and serial and can
be given to `-r` exactly like the
short names. One name, permanent,
immune to plug order.

**One radio, two ports.** Many modern
rigs present *two* serial devices on
one USB cable — an enhanced port for
CAT and a standard one often used for
PTT or CW keying. The capabilities
and the maker's manual say which is
which; Hamlib wants the CAT one for
`-r`, and chapter 6's separate PTT
port (`-p`) is where the other goes.

Check: on a real station you can now
run the three checks — name the
device by plugging it in, read its
group with `ls -l`, find your own
groups with `id` — and state the baud
rate your rig expects. With those
four facts in hand, chapter 4's
session works against hardware, and
chapter 15 has little left to teach
you the hard way.
