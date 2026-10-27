# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's traffic is the lab talking to itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 14 names what the tool
   cannot do: backends of uneven maturity behind one model
   list, features a radio exposes that its backend only partly
   drives, a daemon with no authentication and no arbitration
   between clients, and the legal edge around transmitting
   anywhere a licence does not reach.
8. **The safety rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *the software can key a
   real transmitter: stay inside your licence, your band
   edges, and dummy loads while you learn* — before any
   procedure depends on it, and later chapters point back to
   it where the tooling would make crossing the line easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox.
Hamlib **4.5.5** was taken from Ubuntu's own packages
(`libhamlib-utils` 4.5.5-3.2build2 and the libraries it depends
on). Because system-wide installs are not available on the
build machine, the .deb packages were unpacked into a private
folder and the tools run from there; the bytes are the ones
`apt install libhamlib-utils` lays down, only the location
differs. Every session ran against backends that pretend —
the dummy rig (model 1) and the dummy rotator (model 1) — so
no radio was connected and nothing was, or could be,
transmitted. All transcripts live in
`hamlib-lab/transcripts/`.

Run for real, with transcripts as evidence:

- Version lines of all four tools — rigctl, rigctld, rotctl,
  rotctld — all reporting Hamlib 4.5.5 (t00).
- The model lists: rigctl's list runs to 284 lines (the dummy
  at 1, NET rigctl at 2, the Dummy No VFO at 6), and rotctl's
  to about fifty models, the dummy rotator among them (t01).
- The dummy rig's full capabilities dump, 221 lines: ranges,
  modes, get/set functions and levels, PTT capabilities (t02).
- Interactive sessions with the dummy: frequency read at its
  145,000,000 Hz wake-up value, set to 14,074,000 and read
  back; mode read as FM with a 15,000 Hz passband, set to USB
  2,700 and read back (t03).
- The current-VFO query (VFOA), the signal-strength level
  (which the dummy invents, returning different values on
  consecutive calls — the guide says so), and the power
  status (t03b).
- PTT, both halves of the story: the plain session failing
  with "Feature not available" because the dummy's PTT type
  is None, and the working session with `-P RIG` — `T 1`
  read back as `PTT: 1`, `T 0` as `PTT: 0` (t04, t07).
- rigctld serving the dummy on port 4532, driven by two
  separate rigctl clients using the NET rigctl backend
  (model 2): the second client saw the frequency the first
  had set, and a third keyed PTT through the daemon and read
  it back (t05).
- The daemon's raw text protocol spoken with netcat alone:
  `f` and `\get_freq` answering the bare frequency,
  `F 14060000` answered `RPRT 0`, a nonsense command
  answered with silence, and `\dump_state` reporting the
  dummy's 150 kHz–1.5 GHz range (t05b).
- The dummy rotator: capabilities (Az-El, −180° to 450°
  azimuth), position commands accepted and proven to reach
  the backend under `-vvvvv`, while the read-back stays at
  0.00/0.00 — the dummy accepts positions it never moves to,
  and the guide teaches it as a lesson, not a success (t06).
- Verbose output, including the error ring-dump a failed
  command prints at default verbosity and the
  `returning(-11)` line that named the PTT failure's layer
  (t07).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Every real serial radio: device names under `/dev`, baud
  rates, and opening a rig with `-r`. The build machine has
  no serial hardware at all; the checks it could make are
  recorded (the `dialout` group exists, no `/dev/ttyUSB*` or
  `/dev/ttyACM*` devices are present) in t90.
- The permission fix (`usermod -a -G dialout`) and its
  re-login requirement, and the stable names under
  `/dev/serial/by-id/` (chapter 10).
- The Fldigi and WSJT-X settings screens in chapter 9. The
  Hamlib side they stand on — daemon, protocol, shared
  state, PTT through the port — is fully verified above.
- rotctld: rotctl's local side is verified, and the daemon
  pattern is verified end to end via its sibling rigctld,
  but the rotator daemon itself was not run.
- Running rigctld as a systemd user service; the lab started
  it by hand so transcripts show the bare command.
- Split operation across two VFOs, rotator `\move`, `\park`
  and `\reset`, rig memory features, and building Hamlib
  from source.
- The comparison tools in chapter 13 are characterised, not
  benchmarked.

Fixtures are this guide's own and deliberately generic: a
radio made of software, its factory wake-up frequency, and
band-landmark frequencies any band plan prints (the
20-metre FT8 frequency among them). No amateur callsign
appears anywhere in the guide or its transcripts — the lab
station needed none, because it transmitted nothing — and
no example is modelled on any real person, station,
network, service or organisation.
