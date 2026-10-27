# The Neighbours

Hamlib's job — translating between
programs and radios — is done by
several other tools too, each with a
different centre of gravity. This
chapter characterises them; it does
not benchmark them, and a station is
allowed exactly one owner per rig
(chapter 8's rule) whichever camp it
picks.

## flrig: the neighbour with a face

**flrig** is a rig-control program
with a proper graphical front panel —
tuning knob, meters, memories — that
other programs can drive through its
own XML-RPC server. It sits in
Hamlib's world twice over: Fldigi can
use it as its rig controller
(chapter 9), and it appears on
Hamlib's own model list (model 4) so
a Hamlib client can even talk to a
rig *through* flrig. Choose flrig
when a human at the screen wants the
big knob; choose rigctld when nobody
needs a window and the rig should
just be *available*. The two are
rivals for the serial port and
allies in every other way.

## The makers' own software

Every major manufacturer ships a
control program for its radios —
remote panels, memory editors,
firmware updaters. These speak the
native dialect with no translation,
expose the maker's newest features
first (a Hamlib backend can lag a
new radio by a release or a season),
and typically serve one radio, one
computer, one program at a time.
Memory programming in particular —
filling a handheld with repeater
lists — often belongs to the maker's
tool or to CHIRP (which has its own
guide in this series) more than to
Hamlib, whose memory support varies
by backend.

## Programs that contain their own CAT

Some software skips Hamlib and speaks
to radios directly — satellite
trackers that grew their own drivers
(GPredict has both its own and a
rigctld client), SDR programs that
*are* radios (their "rig control" is
an API for tuning a receiver that
lives in the same machine), and
contest loggers with a cherished
hand-rolled driver for one beloved
rig. None of this is wrong; it is
the world as it was before Hamlib,
kept where it still fits. The trade
is the one chapter 1 opened with:
every dialect learned once, or
learned by everyone.

## The library itself, from code

Everything rigctl does is a library
call, and the library has bindings
for C, Python, Perl and others
through its own interfaces. A
Python script can `import Hamlib`
and hold a rig object whose methods
are chapter 5's commands with dots
in. When the recipe outgrows shell
pipes (chapter 12), this is the
door — with the daemon still
available underneath, because the
NET rigctl backend is a library
feature, not a rigctl one.

## The other direction: radios on the network

A growing family of radios are
network citizens themselves — they
serve their own control protocol over
Ethernet or Wi-Fi, and the shack
computer is just another client.
Hamlib meets them as backends whose
"device" is an address. The shape
rhymes with rigctld so closely
because the industry learned the
lesson this guide teaches: one radio,
many programs, a protocol between.
