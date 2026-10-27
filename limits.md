# Limits

What Hamlib cannot do, and what it
does less well than its reputation —
stated plainly, because a tool you
share a transmitter with deserves
honesty more than enthusiasm.

## Three hundred backends are not one backend

The model list is a census, not a
promise. Backends are written by
volunteers who own the radios, and
they carry grades — Stable, Beta,
Alpha, Untested — that the list
prints and users skim past. A Stable
backend usually does everything its
radio can do. An Alpha one may tune
and nothing else, or tune and then
misbehave at the edges: an unusual
VFO arrangement, a mode the maker
added in a firmware update. The
capabilities dump describes what the
backend *intends*; chapter 11's
verbose mode is how you learn what
it does. No other chapter of this
guide can settle that for your
radio, and none pretends to.

## The dummy is a polite fiction

This guide's instrument teaches the
protocol faithfully and the physics
not at all. Its rotator twin accepts
positions it never moves to; its
signal meter invents readings; its
capabilities claim a kitchen sink no
real radio offers. A reader who
learned only here knows exactly how
to *ask* — and must still learn,
from a real rig at low power into a
dummy load, what answers feel like
when cables are loose and bands are
noisy. The guide says plainly, here
and in `criteria.md`, which is which.

## rigctld does not referee, authenticate, or forgive

One daemon, one rig, no accounts, no
passwords, no audit trail, and no
arbitration between clients
(chapter 8). It is a shack tool, not
a network service, and exposing it
beyond a trusted network hands your
transmitter to strangers in a way no
setting inside it can prevent. It
also holds no opinions about safety:
a client can ask for any frequency
the rig accepts and key it there.
The licence is the operator's; the
software assumes an operator exists.

## Receive is easy; transmit is where dialects bite

Reading a frequency works on nearly
every backend ever written. The
features that fail in the field are
the compound ones: split operation
across two VFOs, PTT by methods
other than CAT, preamp and filter
settings whose names differ subtly
by maker. A program that works
beautifully on receive can still
surprise you on the first automated
transmission — which is why this
guide's safety order (dummy, receive,
dummy load, low power) is written
the way it is, and why it is not
skippable bravado but the tool's
actual shape.

## What Hamlib never had

It has no opinion about audio: the
sound of digital modes travels a
different cable (or the same USB
cable's other device) and is set up
in the programs, not here. It does
not program memories well across the
board, does not update firmware,
does not decode a single dit. And
it cannot repair the physical layer:
a counterfeit USB-serial chip, a
driver the OS dropped, a cable that
charges but carries no data — these
arrive dressed as Hamlib errors and
leave by the hardware door they came
in.

## The edge that is legal, not technical

Nothing in Hamlib checks a licence.
It will tune where a rig can tune
and key where a client asks, across
band edges and licence classes
alike, because receiving is lawful
nearly everywhere and the tool
cannot know who is asking. The
responsibility this creates is the
operator's whole and undivided —
chapter 1's rule, arriving at the
end of the book exactly where it
started: **stay inside your licence,
your band edges, and dummy loads
while you learn.**
