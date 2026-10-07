# Fixtures & patch

Patching fixtures, building groups, and the full channel reference for every built-in profile.

## Patching

**Fixtures tab ▸ Patch.** Each fixture has a profile, a universe, a DMX start address (1-based, as printed on the fixture's display), and a position for the previz. Address ranges that overlap another fixture on the same universe turn red. `+ add fixture` picks the next free address automatically.

Place fixtures by dragging them in the **2D previz** (top-down plan; x is stage left→right, the bottom edge is the audience). Height (`Y`) is set numerically in the patch table — rigged fixtures at ~3 m aim down-stage in the 3D view automatically.

## Groups

Looks target **groups**, and group members are *heads*, not fixtures — a 4-par bar contributes four independently-controllable heads. Toggle membership with the chips in the Fixtures tab. **Chip order is chase order**: a chase effect runs across the group's heads in exactly this order, so "Bar Pars L→R" ordered bar1-p1 … bar2-p4 sweeps the stage left to right.

## Universes

**Output tab.** Each universe has:

- **Art-Net universe** — the 15-bit port-address exactly as your node expects it (0-based on the wire; this rig's node listens on universe 1).
- **sACN universe** — 1-based, if sACN is enabled.
- **Priority** — the sACN priority (0–200) on this universe's packets; 100 is the default and what a grandMA sends. A node that merges by priority takes the higher sender, so when LIGHT shares a universe with a console, set LIGHT above 100 where LIGHT should win and below where the console should. (The grandMA3's own sACN *input* ignores priorities; this is for nodes and fixtures.)
- **Merge in** — take another sender's Art-Net for this universe (a grandMA sharing the rig) into what LIGHT sends, channel by channel, highest wins, so the node hears one source and the DMX monitor shows the merged frame. The other sender must broadcast, or send to this computer's address, on the same Art-Net universe. The button reads *waiting* until frames arrive and *live* while they do; what arrived is dropped 2.5 s after the sender goes quiet, and blackout still wins. LIGHT does not answer ArtPoll, so a console that sends only to nodes its poll has found (a grandMA3 in its automatic mode) will not find LIGHT on its own: set it to broadcast, or to this computer's address. Art-Net only: sACN is not taken in.
- **Destination** — empty = Art-Net broadcast (255.255.255.255) / sACN multicast; or enter your node's IP for unicast (IP literal only). **If the Mac is on Wi-Fi and the rig on Ethernet, set the node's unicast IP** — broadcast follows the default route (usually Wi-Fi) and the rig would hear nothing; unicast always routes out the correct interface.

Output runs continuously at 40 Hz per enabled universe. The DMX monitor at the bottom shows live channel values; engine health (refresh rate, tick jitter) sits above it.

The default architecture this app was built around: **universe 0** carries Resolume Arena → Octostrip pixel data directly (Arena's Advanced Output), **universe 1** is LIGHT's — derbies, bars, hazer via the Art-Net→DMX node. Default patch: Derby1 @001 · Derby2 @011 · Bar1 @021 · Bar2 @051 · Hazer @101.

## Built-in profiles

### Varytec LED Derby ST — 4 Channel

A macro-colour derby: it cannot mix RGB. LIGHT's look editor still shows a colour control — the engine quantises your colour to the nearest macro below (or pick one explicitly).

| CH | Function | Values |
|---|---|---|
| 1 | Colour macro | see table below |
| 2 | Strobe | 0–5 open · 6–255 slow→fast |
| 3 | Motor | 0 off · 1–127 static aim · 128–255 rotate slow→fast |
| 4 | White LED ring | 0–9 off · 10–179 strobe patterns 1–17 · 180–255 full on |

The ring is **on/off hardware** — no proportional dimming exists. LIGHT sends 220 for "ring blinder", or a value in 10–179 for "ring FX" patterns.

Colour macro bands (LIGHT transmits the band midpoint):

| DMX | Colour | | DMX | Colour |
|---|---|---|---|---|
| 0–5 | off | | 126–140 | green + white |
| 6–20 | red | | 141–155 | blue + white |
| 21–35 | green | | 156–170 | red + green + blue |
| 36–50 | blue | | 171–185 | red + green + white |
| 51–65 | white | | 186–200 | green + blue + white |
| 66–80 | red + green | | 201–215 | red + green + blue + white |
| 81–95 | red + blue | | 216–230 | colour change 1 |
| 96–110 | red + white | | 231–255 | colour change 2 |
| 111–125 | green + blue | | | |

Auto-quantisation only picks static bands; the two colour-change programs are explicit-only. "Off" doubles as the derby's blackout — the profile sends 0 when the look's dimmer is at zero.

### KAM Power Partybar WFS — 20 Channel

Four RGB pars on a T-bar, each par an independent head:

| Offset | Function |
|---|---|
| +0/+1/+2 | Par N red / green / blue |
| +3 | Par N dimmer |
| +4 | Par N flash — 0–5 steady · 6–255 slow→fast |

Par N starts at offset (N−1)×5 from the fixture address. LIGHT drives colour on the RGB channels at full and intensity on the dimmer channel, so colour crossfades stay clean at any brightness.

### Generic Hazer — 2 Channel

| CH | Function |
|---|---|
| 1 | Haze output |
| 2 | Fan speed |

The top-bar haze slider writes here directly (merged highest-wins with any look that sets haze).

### Generics for growth

| Profile | Channels |
|---|---|
| Dimmer | 1: dimmer |
| RGB Par | 3: R, G, B (intensity folded into colour) |
| RGBW Par | 4: R, G, B, W |
| Moving Head RGBW | 10: pan, pan fine, tilt, tilt fine, dimmer, strobe, R, G, B, W (16-bit position) |

## How looks become DMX

Per 40 Hz tick the engine resolves every head's parameters (layer merge → effects → masters), then each profile renders parameters to its channels: masters scale dimmer/white before rendering; profiles without a dimmer channel fold intensity into their colour channels; banded channels (derby macros, motor modes) snap rather than fade. The maths is identical in both engines and locked by the parity test.

## Importing a whole design (MVR)

The same import button accepts **.mvr** scene files (exported from
Vectorworks, Depence, grandMA, and most planning tools): fixtures arrive with
their patch addresses, plan positions, and fixture types (the GDTFs embedded
in the file), plus one group per MVR layer. You choose merge (keep the
current patch) or full replace on import. Universes named in the file that
don't exist yet are created automatically. Conventions: positions convert
from MVR millimetres/Z-up to LIGHT metres/Y-up; addresses accept both the
absolute and `universe.channel` forms — exporters vary, so check the patch
table after a first import. Each fixture's **number** comes with it: the
file's FixtureID (a console's fixture number) when it has one, its
UnitNumber otherwise, and it shows in the table's first column, on the plan
and in find. A fixture the file patches on more than one address break is
patched on the first and named in the import message, because LIGHT
patches a fixture once. Only the yaw of a fixture's matrix is read: which
way a fixture faces at rest is LIGHT's own rule, from its height, and MVR's
frame (a fixture standing on the floor) has no verified mapping onto it, so
mounting tilt and roll are set in the table, not read from the file. A
focus point in the file is not read either.

## Exporting the patch (CSV)

**export patch** in the Fixtures tab saves the patch as a CSV file, one row
per fixture in patch order: number, name, manufacturer, model, mode,
universe (its name and its Art-Net and sACN numbers), address, channel count,
position in metres and mounting angles in degrees. It is what a console's
patch importer or a spreadsheet wants; grandMA3 reads one through the
community patch importer, and any desk that takes a CSV maps the columns
by name. An MVR export, which would carry the fixture files as well, is not
offered yet.

## Importing fixtures (GDTF)

For anything beyond the built-ins, click **⇩ import .gdtf** in the Fixtures tab and pick a fixture file (e.g. from [gdtf-share.com](https://gdtf-share.com)). Every DMX mode in the file becomes a selectable profile (marked ⇩ in the dropdown), stored inside the project so it travels with your show. Supported in v1: dimmer, RGB(W) colour, 16-bit pan/tilt, shutter/strobe, and colour wheels (with automatic nearest-colour quantisation, like the derby). A channel the importer has no meaning for holds the fixture's own default until a look sets it: every such channel comes through as a raw fader on the look editor's **Other** tab, named from the file and carrying the chart's band names, so a fixture's zone modes, pattern selectors and control channels are driven even though LIGHT does not know what they mean. A channel the importer holds on purpose (a face's master dimmer at full) is not offered. Both engines interpret imported profiles through one shared implementation, and the parity suite covers it.

**Pixel arrays.** A fixture with many emitters is usually written the way the
GDTF spec intends: the colour channels are declared once on a template lens,
and each physical pixel is a *GeometryReference* to that lens carrying its own
position and a DMX offset per break. The importer expands those references,
so a Robin Spiider comes in as its nineteen pixels in two rings plus the
flower, on every mode, with the footprint the fixture actually occupies —
a Pixel RGB mode is 91 channels, a Pattern full RGBW mode is 123. A reference
lists one offset per break; a channel declared on a numbered break takes the
matching entry, and one declared as an *Overwrite* takes the last, which is
how one file serves both its three- and four-channel modes. Both spellings
of the position matrix are read: the spec's 4×4 with the translation in the
last column, and the axis-vectors-plus-origin form some consoles export. The
face is laid out in whichever plane the pixels actually lie in, so a bar or
panel reads across and up as before and a moving head's face reads as a
face, not a line.

**Levels on a layered face.** A bar whose cells each have a dimmer (a
COLORado PXL Curve) dims on those, and its master dimmer is held at full so
the bar is not dimmed twice. A face whose pixels have no dimmer of their own
beside layers that do (a Spiider's nineteen pixels under a flower and a
pattern layer, each with a dimmer, and a master over all of it) is read the
other way round: every dimmer is held at full and the colours carry the level,
so the look's dimmer reaches every pixel and a dimmer wave across the face
reaches the wire. A filter picked on such a fixture's colour wheel darkens the
pixels so the filter shows, and the dimmer of the layer it colours carries
the look's level for as long as it is picked. The same holds for any face
whose pixels have no dimmer of their own, a plain RGB batten under a master
included. A pixel that is a dimmer and nothing else (a CLF Nero's 28 white
beam segments, a sunstrip's lamps, a blinder's cells) is a head of its own,
a white head that follows its own level, when the fixture has two or more of
them; one alone is the fixture's dimmer. A background colour table's level
is the dimmer that goes with the table: the one on the table's geometry,
else the next dimmer the file lists after it. Every shutter follows the
look's strobe, a flash-rate channel written as `StrobeRate` included.

**More than one DMX break.** A fixture that keeps some channels on a second
break (a pixel section addressed apart from the main body) is laid out as one
block, break after break:
patch its breaks back to back on the fixture and the addresses line up. The
footprint is the whole block.

**Files the importer forgives.** `description.xml` is found in any folder or
spelling of case inside the archive; UTF-16, Latin-1 and byte-order-marked
files read; a 24-bit channel is driven on its two coarse bytes and counted
whole; two modes whose names slug to one id are numbered rather than the
second replacing the first. A description over 64 MB, or an archive with no
description at all, is refused with a message, and an importer error of any
kind is a message rather than a stuck import queue.

A profile compiled by an older build carries its compiler version, and the
Rig view flags it with **rebuild from library** when the importer has learned
something since. Take the offer: a pixel array compiled before this could have
been short of both heads and channels, and the address after it may need to
move; a profile compiled before the raw faders has no **Other** tab until it
is rebuilt.
