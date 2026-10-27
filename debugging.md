# Debugging with Verbose Output

This chapter ends with you able to
make Hamlib show its working — and,
more importantly, able to read what
it shows: which layer was reached,
where the call failed, and what the
negative numbers mean. When a station
misbehaves at a contest's worst
moment, this chapter is the one open
on the second screen.

## Turning the lights up

Every tool takes `-v` for verbose,
and it stacks — each repetition
raises the level, up to five:

```bash
rigctl -m 1 -v        # a little
rigctl -m 1 -vvvvv    # everything
```

At the top level the library narrates
its life: backends initialising, the
rig opening, each command entering a
function in the library, the call
crossing into the backend, and the
return coming back. For the lab's
simple `f` against the dummy, the
chain reads (transcript t07):

```text
initrigs4_dummy: _init called
 1:dummy.c(224):dummy_init entered
 1:rig.c(817):rig_open entered
rig_open: cwd=/home/hatch
  2:dummy.c(339):dummy_open entered
   3:dummy.c(1506):dummy_get_powerstat entered
   3:dummy.c(1509):dummy_get_powerstat returning(0)
```

Read it as an audit trail. The
indentation is depth: library calls
backend, backend answers. Every line
ends in `entered` or
`returning(<code>)`. Zero is success.
A negative code is an error, and the
function whose `returning` line first
goes negative is where the failure
lives — library side, backend side,
or (on real hardware) the wire
between, whose bytes verbose mode on
a serial backend also prints, as hex,
in both directions.

## Errors explain themselves, loudly

One Hamlib behaviour surprises every
newcomer: a *failed* command at
ordinary verbosity dumps a compressed
trace of its own — Hamlib keeps a
ring of recent calls, and an error
prints the ring. Chapter 6's PTT
failure did exactly this, and its
tail carried the verdict in plain
words:

```text
 2:rig.c(3587):rig_get_ptt returning(-11) Feature not available
 1:rigctl_parse.c(2583):rigctl_get_ptt returning(-11) Feature not available
Feature not available
```

The codes you will actually meet:

| Code | Name | Usually means |
|------|------|---------------|
| 0 | success | — |
| −1 | Invalid parameter | a value the backend rejected: wrong VFO, wrong mode spelling, PTT with no type |
| −2 | Invalid configuration | a setting that cannot be combined as asked |
| −4 | Memory allocation / not implemented family | rare; note the exact text for a bug report |
| −5 | IO error | the serial device vanished, timed out, or is not answering |
| −6 | Internal error | a bug; capture the trace |
| −9 | Protocol error | bytes came back that the backend cannot parse — wrong baud rate is the classic cause |
| −11 | Feature not available | the capability is not there — or not switched on, as PTT was not |
| −17 | Invalid operation | the rig is in a state that forbids it (for example, setting while transmitting) |

Keep the table short in memory by
keeping the habit instead: read the
*words* beside the last negative
number; Hamlib almost always names
its own failure correctly.

## Where to point the verbosity

- **rigctl first.** Before blaming a
  program, reproduce its operation in
  rigctl at `-vvvvv`. If rigctl
  succeeds, the program's settings are
  the problem; if rigctl fails, the
  program never had a chance.
- **The daemon's own output.**
  rigctld started in a terminal prints
  its errors there. Verbose flags at
  its start raise the same narration,
  and one daemon run at `-vvv` tells
  you which client asked for what.
- **The raw port.** Chapter 8's
  netcat test splits the world in
  two: protocol answers mean the
  Hamlib side is healthy, whatever
  the GUI says.

## Writing a bug report worth answering

Backends are maintained by volunteers
who own the radios. A report that
gets fixed contains: the model
number, the exact command, the
`-vvvvv` trace from a minimal
session (the failing command and
nothing else), and the version line.
The lab's PTT surprise would have
made a poor report — the trace itself
showed the missing PTT type, which is
the general rule: read the last
twenty lines before you write the
first one.

Check: reproduce chapter 6's failure
at default verbosity and at `-vvvvv`,
find the returning(−11) line in
both, and say in one sentence which
layer returned it. That sentence is
the skill this chapter exists for.
