# User guide

How to operate LIGHT: looks, layers, cues, effects, and the controls that matter mid-set.

## Getting started

A rig needs five things set up before it lights, and LIGHT walks you through
them. The steps appear by themselves on a show with no fixtures — which is what
**New project** gives you — and **Settings ▸ Rig setup** brings them back. Each
one ticks itself off by reading the show, so anything you have already done, or
that arrived in an MVR import, is ticked before you get to it.

1. **Output** — switch on Art-Net or sACN for the universes your nodes listen to.
2. **Fixtures** — patch what is on the truss, at the addresses it is set to.
   Import a GDTF for one fixture or an MVR for the whole plot.
3. **Stage** — set the size of the room and drag the fixtures on the plan to
   where they hang. This is what the stage view draws and what spread effects
   fan across.
4. **Groups** — the fixtures you will light together. Looks are built on groups,
   so nothing can be programmed until there is at least one.
5. **Aim** — point each moving head at the stage once. Looks then move around
   that aim rather than from the centre of its travel.

Then **go live**. LIGHT starts offline every time it opens and sends nothing
until you say so, so the last step is deliberate: click **offline** in the top
bar, or **go live** in the Output tab, and the universes you switched on start
transmitting. Going back offline blacks the rig out first, then stops — a node
holds the last frame it was sent, so falling silent on its own would leave the
rig lit.

Offline is not blackout. Blackout is the show being dark and is still
transmitted; offline is LIGHT not speaking to the network, while the show keeps
running on screen.

A first launch skips all of this: it opens the demo show, which is already
patched, grouped and staged. Fire some pads and look around.

## The mental model

- A **look** is a lighting state: colour, intensity, positions, and effects for one or more fixture groups.
- Looks live in a **grid**: rows are **layers**, columns are **cues**. The layer stack runs bottom-to-top (WASH at the bottom, STROBE on top — the UI shows the top of the stack as the top row). Four layer rows, plus a fifth **control row** ruled off underneath them — the same shape as an APC40 mk2's clip grid.
- Clicking a pad fires its look on that layer with a crossfade. Clicking a **column header** fires the whole column as a cue.
- Everything time-based (effects, fades shown in beats) follows the **beat clock** — tap it, drag it, or let Resolume drive it.

## Firing looks

| Action | How |
|---|---|
| Fire a pad | Click its body (also selects it for editing) |
| Select without firing | Click the pad's **name strip** — useful mid-show |
| Move a look to another pad | Drag the name strip onto the other pad |
| Duplicate or clear a pad | Right-click the name strip (long-press on a touch screen) |
| Fire a column (cue) | Click the column header, or keys `1`–`8` |
| Hold a flash look | Press and hold the pad — it releases on mouse-up |
| Clear a layer | `✕` in the layer header |
| Rename or bypass a layer | Click the layer's name (hold on glass) and pick from its chooser |
| From MIDI | Map pads/faders with MIDI learn (below) |
| From Resolume | Enable OSC output in Arena — column launches follow automatically |

**Column = cue.** Firing a column fires every layer's pad in that column and *clears* layers whose pad is empty — so a column fully describes the stage. Momentary **flash** looks are skipped by cues on purpose: a cue can never latch a blinder on.

**Flash looks** are momentary: active only while the mouse button or mapped MIDI note is held. If the client holding a flash look disconnects entirely, the engine releases it automatically.

**Reading a pad.** The block above the name is a miniature of the rig: one mark
per group the look touches, left to right in stage order, in the colour that
group will take. Each mark also says what its group does. The fill rises to the
look's brightness inside an outline (a solid block is full; a thin base line
just inside the outline is a dimmer at nearly nothing); a rainbow fill is a hue
effect; a diagonal hatch is a wave on the brightness, white or strobe, and
horizontal bands are a chase — one band lit in two, three or four, the way one
head in that many is lit at once, so the more of the run a chase lights, the
fewer the bands; a hollow grey hatched outline is a look that is only an
effect, or a modifier — the two draw alike, since neither has a colour of its
own (a modifier's is a change to what is under it), and the tooltip says which;
a dot on a mark is a part that aims or spins a head (at the top, or at the
bottom of the last mark where the corner would cover it); and a row of dots
along the bottom left is a steps look.
The corner carries at most two marks: the bolt for a flash look, and one glyph
for what the look does over time — a burst for a strobe, and the wave for a
chase, a square, a random, a ramp or a sine. Hover a pad and the tooltip says
the same word. Library tiles draw the same face, only shorter, so the hatch
and the bands give way there and the corner glyph carries the effect.

**The name strip is the handle.** A pad's body fires it, and the strip along the
bottom with the look's name on it does everything else: click to select without
firing, drag onto another pad to move it there (in Build, where one layer shows at a time, drop it on a layer key — L1 to L4 — to move it to that layer in the same column), right-click for the menu.
Dragging onto a pad that already holds something swaps the two rather than
replacing, because rearranging is the reason to drag.

**Duplicate versus a second pad.** Looks live in one shared pool and a pad only
points at one, so putting the same look on two pads is not a copy — edit either
and both change. That is usually what you want. `Duplicate` in the pad menu is
the other thing: an independent copy on the next free pad in that layer, which
you can change without touching the original. The library drag does the first,
the menu does the second.

## Crossfades

Each layer has a default fade (seconds) in the project; a look can override it with its own **fade** field in the look editor. Colours fade through RGB space (exactly what the fixture's channels do), intensities fade linearly, and *banded* values — derby colour slots, motor modes — snap at the start of the fade because the hardware can't fade between bands.

The **fade** fader in the top bar stretches or shrinks every crossfade from the moment you move it, without editing a look: at the bottom (`cut`) every pad, column and clear lands at once; in the middle (`1.00×`) each look fades the way it was written; at the top (`4.00×`) they take four times as long. It applies to what fires next: a fade already running keeps its length. A flash still lets go over at least 20 ms, so it never clicks. Like speed, it is not saved with the show, so a show always opens as programmed. Double-click it to put it back in the middle, or type a number (`2` is twice as long).

## Layers and blend modes

Layers apply bottom-to-top. Each has a **master** (scales that layer's intensity contribution) and a **blend mode**:

- **replaces** — replaces what's below on the channels the look touches. For base washes.
- **dims** — scales the intensity (dimmer/white) below. This is the FX layer's mode: a chase or pulse modulates *whatever colour the wash is showing* without owning colour itself.
- **brightest** — the brighter of the two layers wins, on intensity. For strobes and blinders that sit on top.

To change a layer's blend, click its name (or the blend word beneath it) and
pick from the chooser; right-click or, on glass, a hold does the same. The
same chooser holds **Rename…** — the head reads `L1` to `L4` while a layer
still has its default name, and the name you give it (`WASH`, `FX`, `STROBE`)
once it has one — and **bypass**. A locked client cannot change the blend or
the name: its chooser opens with bypass alone in it, because that is a
performing move and the lock never stops the show.

**Bypass** is Arena's layer **B**: pick it and the layer goes silent instantly;
the same item now reads **bring back**, and picking it — or clicking the amber
`bypassed` word on the head — returns the row. Its pad keeps playing the whole
time — the live look, the crossfade, the column, a held flash are all exactly
as they were, and the rows above sit directly on the rows below — so you can
take a row out under pressure without losing what was on it, and un-bypassing
returns the exact frame the row would have been showing. While bypassed the
head's name and the line under it turn amber and read `bypassed`; on the phone
the row's number turns amber instead, and a tap on it brings the row back. A
bypassed row is also listed in the **held** chip in the top bar, on every page
and on the remote, with a `bring back` beside it. The row's `✕`, a column and
a flash release leave a bypass alone; ALL STOP and opening another show clear
it. It is never saved, so a show always opens with every row heard. The
audition lifts a bypass on the row it auditions on, as it lifts the master and
blackout — a modifier auditioned over a bypassed row shows nothing beneath it,
exactly as on stage.

**Modifier looks.** A look with **modifier** ticked (in the look editor, beside
*flash*) is Arena's effect clip as a property of the look: it does not paint
the rig, it *shapes what is under it*, on any row and whatever that row's
blend says. Its values are read as deltas from each parameter's neutral —
brightness dims the composite below, colour turns its hue (a white wash stays
white), aim and beam swing around wherever the wash has put them, strobe adds
the faster shutter and white adds the brighter white. The row's master still
scales a modifier's brightness bite, so a fader at zero leaves the wash's
brightness alone — a colour turn, an aim or beam swing, a strobe and a white
are not a master's business, on this row as on any other; the row's blend does
not apply to it. Over an empty row a modifier shows nothing, because there is
nothing to shape. The audition previews a modifier over whatever is playing
beneath its row, the way Arena previews an effect over the clip that is
playing. A *steps* look is a modifier through the steps it plays, so the
toggle is not offered on one.

**Group levels** are a fader per group on the row under the dials: pull a group
down without touching a look. The lowest level over a head wins rather than
multiplying, because auto-groups put most heads in two groups. They are never
saved and ALL STOP clears them — a fixture that must stay out of the show is a
mute instead. Click **GROUPS** at the head of the row to pin groups: pinned
groups come first, in the order you pin them, and the **APC40 mk2 · busk**
controller layout puts the first four on its track faders 5–8.

The **grand master** (top bar) scales all dimmer/white output, and a fixture's background level with it. **Blackout** (top bar or `B`) zeroes intensity and strobing instantly and stops a fixture's own effect programs — it always wins.

## The look editor

Select a pad → the Look tab shows its editor. A look is a list of **parts**; each part targets one fixture **group** and carries:

- **Dimmer** — intensity 0–100%.
- **Colour** — hue + saturation faders plus swatches. Derbies can't mix colour: they quantise to the nearest of their 14 fixed colour slots ("auto"), or pick a slot from the dropdown.
- **Derby extras** — *ring blinder* toggle (the white LED ring is on/off hardware — there is no ring dimmer), *ring FX* (the ring's built-in strobe patterns), *motor* (off / static aim / rotate + speed).
- **Optics** (imported moving heads) — *gobo* and *prism* slot pickers built from the fixture's own wheel, a *spin* fader for each wheel that rotates, and the strobe's *pattern* (plain / pulse / random / rise / fall / swell) where the shutter has the bands: rise fades each flash in, fall fades it out, swell does both. Some fixtures (a COLORado PXL Curve) add *random rise*, *random fall*, *random pulse* and *pulse 2*, and with that many the patterns are a list rather than buttons. Not set leaves a wheel where the fixture parks it. A fixture with a centre *flower* effect (a Robin Spiider) gets a **flower spin** fader of the same shape: the middle is still, either end is full speed one way, and not set leaves the effect off. It is nudge-able and an effect target like the wheel spins.
- **White** — the dedicated white emitter on an RGBW head, offered whenever
  something in the group actually drives one. Distinct from a derby's *ring
  blinder*, which is on/off hardware.
- **Strobe** — shutter rate, slow → fast.
- **Position** — pan/tilt for moving heads, and *move speed* where the fixture has a speed channel: fastest at the right, whichever way the fixture's own channel runs.
- **Built-in colour and background** (on the Colour tab, where the fixture has them) — *built-in colour* picks one of the fixture's preset colours, and *background colour* and *background level* set the second colour a pixel fixture shows behind its programs. Picking a background colour brings its level up unless the look sets one.
- **Programs** (imported fixtures with shows of their own) — *move program*, *program* and *program 2* pick from the fixture's own list by name or number, 0 being off, each with its speed and fade where the fixture has them. The fixture plays a program itself, so the stage shows the look around it but not the program.
- **Haze** — output + haze fan for hazer-type fixtures (merged highest-wins with the manual haze slider in the top bar).
- **Other** (imported fixtures) — one fader for every channel the fixture has that LIGHT has no fader of its own for: a Spiider's zone and pattern channels, a MegaPointe's beam shaper, a control channel. Each reads as the DMX value it sends and the name of the band it lands in, from the fixture's own chart; where the chart has more than one band, a dropdown beside the fader jumps to a band's start. The value goes to the fixture as it is, so the stage view does not show it, a nudge cannot move it, and an effect cannot ride it. The tab appears only when something in the group has such a channel. A fixture imported before this build shows nothing here until the Rig view's **rebuild from library** has run on it.

Enable a parameter with the checkbox to its left; a look only writes the parameters it has enabled, which is what lets layers combine cleanly.

### Effects

Each part can stack effects. **browse…** opens a catalogue of ready-made ones — grouped, searchable, and applied as you click so you can audition down the list — or build your own: an effect modulates one target with a wave, and any fader a fixture has is a target: dimmer, hue, white, strobe, pan, tilt, every beam parameter (zoom, focus, beam size, soften, tint, the blades, the wheel and prism spins, the angles, the shakes), move speed, the program speeds and fades, the background level. The target menu files them by family, the ones something in the group can take first and the rest below. A target the look never set swings about mid-travel. The wheels and the fixture's programs are targets too, stepped rather than swung (see *Wheels and programs* below). The exception is **shape**, which drives pan and tilt together to trace a circle, a figure of eight, a square or a path you draw (see *Drawn paths* below):

| Wave | Feels like |
|---|---|
| sine / triangle | smooth swells |
| ramp up / ramp down | builds / beat-pulses |
| square | on/off gate (set *width* for duty) |
| chase | one-at-a-time run across the group (*width* = how many are lit) |
| random | sample-and-hold flicker |
| curve | one you draw: drag its points, bend a segment by its middle, and the value holds after the last point until the cycle restarts — a fast ramp then a level held to the end, which no fixed wave can do |
| steps | a short list of levels you type, two to sixteen, each held for an equal share of the cycle: a four-step pulse to start with. *Snap* jumps between them, *ramp* moves evenly, *smooth* eases in and out |

Every effect row draws one cycle of its wave in a strip beside the wave picker, with a mark in the live colour riding along it as the rig plays — the picture is computed the same way the engine computes the output, so the two cannot disagree. For a curve the strip is where you draw: double-click to add a point, right-click (or alt-click) to remove one, and the dashed run after the last point is the hold.

**Wheels and programs.** A gobo or prism wheel, a colour wheel, the fixture's built-in colours and background colours, and its own programs are targets an effect steps through rather than swings, because a wheel has no halfway. Pick one and the row shows a chip for every slot the group's fixture names: tick the ones the effect should visit, and the wave says which is up — a ramp walks them in wheel order, a sine walks them there and back, a square shows the two ends, random picks any, and *steps* walks them in order. *Size* and *mix* only gate it, and the fixture's own change time decides how fast each slot arrives, so a four-slot gobo walk at 1 bar is a slot a beat.

**Drawn paths.** Pick *drawn path* as a shape's figure and a pad opens beside it, starting as a triangle. The heads visit the points in turn at an even speed and come back to the ringed one, where the lap starts, so a long side takes longer than a short one. The lines between points are straight, so every point is a corner the heads visibly hit; add points where you want a curve to be rounder. Drag a point to move it, double-click a side to put a new point in it (up to 32), and right-click (or alt-click) a point to take it out (a path keeps at least two). The pad shows the path before *size*, *aspect* and *turn*, which apply on top as they do for every figure. The fixed figures get a small pad too, with a dot in the live colour riding round them. *Star* in the catalogue is a drawn path to start from. The path is saved with the effect, so it goes into the FX pool and to other shows with it.

- **rate** is musical: 1/4 beat up to 8 bars.
- **size** is depth.
- **spread** fans the phase across the group's heads — spread on a saw = a wipe; chase forces full spread.
- Chase/spread order = the order of heads in the group (see the Fixtures tab; chip order is chase order).

The **speed** fader in the top bar multiplies all effect rates (0.25×–4×) without jumping their phase.

**An effect as a modifier.** The FX row's *dims* blend is one way to run a chase
over a wash; a **modifier look** is the general one. Build a look with no
parameters enabled and one effect — a dimmer chase, a hue sine, a tilt sweep —
tick **modifier** in the look editor, and put it on the row above the wash. It
then runs *on* the wash beneath it: a dimmer chase dims it, a hue effect turns
its colour, a tilt effect swings its aim about where the wash aims, a zoom
effect swings the beam about mid-travel when nothing below has zoomed, a white
pulse adds white on top. Three things to know: over an empty row nothing shows
(a modifier has nothing to shape); over a white wash a colour turn does nothing
(white has no hue to turn); and the row's blend does not apply to a modifier —
it does the same thing on a *replaces*, *dims* or *brightest* row, and the
row's master scales only how deep it dims. On the *dims* row a modifier dimmer
chase is byte-for-byte what the old effect-only look was, so nothing you have
built changes.

## Tempo

- **TAP** (or `T`) — tap tempo; every tap also lands the beat.
- **The BPM number** takes a typed value: click it, type, press Enter. Escape
  leaves the tempo alone. Dragging it vertically still scrubs, and a drag never
  opens the field.
- **SYNC** — press it on the downbeat. The bar count restarts from there, and so
  does every effect cycle: a shape that takes a bar to go round starts again at
  the top rather than wherever it happened to be. Arena's own resync over OSC
  does exactly the same thing.
- With Resolume connected via OSC, Arena's tempo drives the clock (see [resolume-and-midi.md](resolume-and-midi.md)).

## MIDI learn

1. Click **MIDI LEARN** in the top bar (it arms).
2. Click any pad, column header, layer master, or top-bar control.
3. Touch the control on your device — pad or encoder. Done; the mapping is stored in the project.

**Colours** (settings → Display) switches this screen between the dark desk and a light one for daylight — a laptop outside, a phone at a festival. It is saved on the screen you set it on, not in the show, and the stage stays dark either way.

Every MIDI input LIGHT can see is a key in **Sync · MIDI**, lit while LIGHT listens to it. A controller that belongs to another app on the same Mac — Resolume's own APC — goes off there, or its pads fire LIGHT's cues too; see *Two controllers on one Mac* in the Resolume and MIDI guide.

Notes fire pads (note-off releases flash looks); CCs drive faders. Manage or delete mappings in the **Sync · MIDI** tab.

A MIDI clip in a DAW can fire pads the same way. While the app is running it
publishes a MIDI port called **LIGHT**, so it is already in your DAW's output
list with nothing to configure; an IAC bus works too if you would rather route
one. [resolume-and-midi.md](resolume-and-midi.md) has the details, and the two
things to know before building a show on it: a mapping points at a position
rather than a look, so it follows the song, and nothing chases a timeline.

A control in the grid's control row is learnable the same way — arm learn, click
its fader, touch an encoder. Prefer an encoder or fader to a pad: a pad drives a
continuous target by its velocity on press only (so a release cannot slam a dial
to zero), which means a pad can push a control up but never bring it back down.

**On the APC40, CLIP STOP is GO.** The row of eight buttons directly under the
clip grid fires whole columns: CLIP STOP under column 3 fires LIGHT's column 3,
the same thing the column head on screen does. It says *stop* because Ableton
made it a stop button and Arena taught you to read it that way — here it starts
a section, and it is the only row on that surface that can hold the cues without
taking the dial row underneath the layers. `METRONOME` is SYNC: it calls the
moment you press it the top of the bar, for the tempo and for every effect
shape, exactly as the SYNC button on screen does. Both arrive with the APC40 mk2
layout; load it once and the surface runs the set without the laptop.

**The APC40's eight device knobs drive the eight controls.** Load the APC40 mk2
preset in the Sync · MIDI tab and knob *N* becomes control *N* — the control row
shows which knob it is under each fader, read from the mappings themselves, so a
blank there means nothing is bound yet.

Two things worth knowing about that surface. The knobs are **absolute**, so the
first move jumps the control to wherever the knob is physically sitting — the
same pickup behaviour as the track faders on the layer masters. And in the
APC40's generic mode the knobs are *banked* by the `[TRACK SELECTION]` buttons,
which silently moves them to a different MIDI channel; the preset therefore binds
all nine banks to the same eight controls, so a stray press of a track button
cannot take your dials away mid-set.

**The pads light up.** Both the APC40 mk2 and the APC mini mk2 get LED feedback,
and both can be plugged in at once. A pad glows dim in the colour of the look it
holds and bright in the colour of what its layer is actually playing; a layer
button lights while that layer has something to clear; the blackout button
blinks while blackout is armed; and the tap button pulses on the beat, so the
tempo is readable on the surface with the house lights down. On the mini the
bottom pad row is the column row, and a column button glows bright only while
every layer holding something in that column is playing it — the whole column
up, which is exactly what pressing it does.

Nothing to configure: LIGHT looks for the surfaces by port name and attaches to
whatever it finds. The packaged app drives the LEDs from the engine; a browser
session drives them itself over Web MIDI, and only one of the two ever writes.

## Saving

Everything autosaves ~1 second after any edit, with five rotating backups (`.bak1`–`.bak5`) next to the project file. `⌘S` (or the save button) forces a save. Live-performance state (which looks are active, grand master, blackout) is deliberately *not* saved — a restart always comes up dark and safe.

## Keys and gestures

Press `?` at any time for the full sheet — two tables, the keys and the
gestures, both rendered from the same lists the app runs, so neither can be out
of date.

**Keys.** `1`–`9` fire columns · `T` tap tempo · `B` blackout · `[` `]` previous / next song ·
`Esc` deselect · `⌘S` save · `⌘Z` / `⇧⌘Z` undo / redo · `⌥1`–`⌥4` Pads / Stage / Rig / Build.
Shortcuts are ignored while you're typing in a field.

**Gestures.** Right-click a pad's name for its menu, a column head to rename,
insert or delete it; drag a pad's name to move the look to another pad (the two
swap). The layer's ✕ stops that layer. Click a library tile and then a pad's
name to place the look; type a name on the plan to select the lights whose names start with it;
hold the lock to unlock the screen. On glass there is no right-click, so every
one of those is a hold instead — and the sheet says so, because it words each
gesture for whatever pointer you are actually using. The full table, both
wordings, is in [the reference](website/10-reference.md#gestures).

---

## Since the first release notes — what else is in the app

**Songs.** The chips under the top bar are pages of the grid, one per
song. Click to switch, double-click to rename, `⧉ duplicate` copies the current
song's pads into a new one (the usual way to start the next song), and the
APC40's bank ◀ ▶ arrows step through them. Looks live in one shared pool, so the
same look can sit in many pads and songs — an empty pad offers
**use existing look…** as well as **+ create look here**.

**Steps (⛓).** Any look can become a chaser: open it and press `⛓ steps`,
then add steps (a look + a beat count each). It hard-cuts through the steps on
the beat, loops, and follows the speed master. Steps cannot nest.

**Undo/redo.** `⌘Z` / `⇧⌘Z`, or the ↺ ↻ buttons. The history lives in the
engine, so every screen — the laptop and the tablet — shares one: whoever made
the edit, ⌘Z steps it back, and the button's tooltip names the step it will
take ("undo rename song “Intro”"). A hundred steps; a drag counts as one.
Edits are steps: pads, looks, songs, columns, the rig, imports, Keep on a
nudge, a learned MIDI mapping. What you play is not — song switches, masters,
haze, nudges and blackout stay where they are when you undo, and undoing an
edit made on another song leaves you on the song you are on. History belongs
to the loaded project: switching projects clears it.

**Projects.** The project name in the top bar is a menu: switch between shows,
`+ new project…`, or `save as…`. Files live beside the app's data; the app
remembers which one you had open.

**Fixtures table.** The first column is each fixture's number — what a plot, a
console and the find box call it; an MVR brings it in, patching gives new
fixtures the next one, and it can be typed. **export patch** saves the table
as a CSV. Number fields (position, mount rotation, tilt, roll) accept typed
values including negatives, or **drag left/right on the field to scrub**. Select
rows first — click, ⇧-click for a range, ⌘-click to toggle, or drag a box — and
any edit applies to the whole selection. The toolbar then offers
`→ universe`, `⇢ re-address`, `⧉ duplicate`, `✕ delete`, and
`⊕ group from N selected`.

**Freeze.** `freeze` in the top bar holds the rig on the frame it is showing while
the show carries on underneath, so a look can be edited with the rig up without
the room watching it being built. The stage view keeps following the edits. It
holds everything, including the channel check tool, and blackout and ALL STOP
release it.

**Head calibration.** `Aim pan` / `Aim tilt` are a moving head's focus, shown as
a percentage and as an angle against that fixture's own travel. `Cal` is how the
head is wired rather than where it points: reverse either axis, swap them for a
head hung on its side, or set limits it may not be driven outside. Nothing is on
by default. Mounting rotation is still not compensated on the wire — if a head
sweeps the wrong way, reverse it here and the rig and the stage view agree.

**Fixture aim.** `Rot°` is yaw, `Tilt°` is the mounting pitch, `Roll°` the roll.
They compose on top of each fixture type's default aim, so a bar hung at an
angle can be pointed where it actually points — visible in both stage views.

**Output adapter.** The Output tab's `output adapter` picker says which network
adapter Art-Net and sACN leave on. `automatic` lets the system choose a route
per packet — and on a Mac with a phone tethered or a VPN up, a broadcast can go
out the wrong one while the app still reads `live`. Choose the adapter your rig
is on (shown with its current address) and every frame is pinned to it. LIGHT
follows that adapter through a new DHCP lease or a cable swap and, if it is
unplugged, sends from any adapter until it returns — the line beside the picker
says which is happening. The choice is remembered on this Mac, not in the show.

**Fixture profiles keep up with the importer.** A profile compiled by an
older version of LIGHT's importer is rebuilt from the `.gdtf` in this
machine's fixture library the next time the show opens — a toast says how
many — keeping any pixel layout you authored and the file's attribution. A
fixture whose file is no longer in the library keeps its old compilation
(the Share panel's `rebuild from library` covers that by hand).

**Art-Net node health.** The `art-net` dot goes green only when a node has
answered an ArtPoll, with its name in the tooltip; amber means LIGHT is sending
but nothing is answering. The Output tab lists the nodes it found. Red and
`sends failing` means the operating system itself refused the packets — the
tooltip carries its error — so nothing is reaching the rig however live the
app looks. On a Mac that is almost always the Local Network permission: allow
LIGHT under System Settings, Privacy & Security, Local Network, then quit and
reopen it (the permission is only read when the app starts).

**Merge in.** When a console shares the rig, switch `merge in` on for that
universe in the Output tab and LIGHT takes the console's Art-Net for it into
what it sends, channel by channel, highest wins, so the node hears one source
and the DMX monitor shows what the rig gets. The console must broadcast, or
send to this computer's address, on the same Art-Net universe. LIGHT
answers ArtPoll as a controller and lists the universes it takes in on, so a
console that sends only to the nodes it has found (a grandMA in its automatic
mode) finds LIGHT by itself; the button reads `waiting` until frames arrive and `live`
while they do. What arrived is
dropped 2.5 s after the console goes quiet, and blackout still wins.

**Ableton Link.** `link` in the top bar joins a Link session (native engine
only) and shows the peer count. Tapping tempo in LIGHT leads the session.
Link is not in 1.0.0 builds, pending Ableton's Link licence: there the switch
is greyed out and says "Ableton Link isn't in this build". A show saved with
Link on still opens, and the tempo follows tap, MIDI beat clock and Resolume.

**MIDI beat clock.** `clock` in the top bar takes the tempo from whatever is
sending beat clock — a CDJ, a DAW, a drum machine. There is nothing to set up:
switch it on and the first input sending clock owns the tempo until it goes
quiet. The button is amber while it is waiting for one and blue while it is
following; the tooltip names the port, and so does the Sync ▸ MIDI tab. While it
is following, the tempo readout turns blue and `tap` greys out — the clock would
overwrite a tap on the next frame, so the button says so rather than doing
nothing. A transport start also lands the downbeat. A stop leaves the tempo
where it was: a source going quiet is a stall, not a tempo change, and a rig
that slowed to nothing because a cable moved would be worse than one holding its
last known tempo.

Link and beat clock are one choice, not two — switching either on turns the
other off. LIGHT pushes a locally-set tempo *into* a Link session, so following
a clock while leading a session would pass that clock's jitter on to every other
machine in the room. Native engine only: a browser cannot see a timestamp worth
averaging. If Arena is also set to drive the tempo, the beat clock wins while it
is following.

**Move, Turn, Select.** The plan's tool picker decides what a drag does: `Move`
places a fixture or a piece of structure, `Turn` swings it (snapping to 5°), and
`Select` only ever changes what is selected — nothing on the plan can move while
it is on. The plan keeps whichever you pick, so a plot that is placed and done
can stay on `Select` for the rest of the tour. Whatever the tool, a *click*
never moves anything: a press has to travel a few pixels before it counts as a
drag, so picking fixtures out to build a group leaves the plot exactly where it
was placed.

**The band.** The `+ musician…` picker in the 2D bar drops dummy performers
on the plan — drag to place, double-click to remove. They appear in both 3D
views (`band` toggles them in-app, `M` in the pop-out window), so you can judge
how a look actually lands on people. `STAGE WINDOW` in the top bar opens the native
window with real beams, haze, and shadows.

**What the stage view draws.** The 3D view in the app draws a look the way
the native window does: the beam's cone and the pool where it lands, with the
profile's own beam and field angles under the zoom the look sets. Beam size
narrows the cone and its pool without dimming them, soften blurs the edge and
widens the pool, the gobo the look picks is drawn on the pool from the
picture in the fixture's file (a file without pictures draws an open beam),
a prism splits the beam into its facets' copies, and both wheels turn at the
speed the look's spin fader asks for, in the file's own degrees per second.
The colour is the colour leaving the lens: warmth tints the beam the way a
lamp at that colour temperature would (from the file's own Kelvin ends where
it gives them), tint pushes it toward green or magenta, the colour wheel shows
its filter, a position between two filters shows the split, and a spinning
wheel turns its filters past the beam. A strobe flashes in the pattern the
look picks (pulse, rise, fall, swell and the random ones) when the fixture
has that pattern. Framing blades cut the beam where they are put, each end
on its own so a blade can slant, and the whole set turns with the blade
rotation (45 degrees each way in the picture). In the app the cut shows on
the pool and the shaft; the native window cuts the shaft only, since its
pools are real spotlights. The animation wheel scrolls the picture in the
fixture's file across the beam, along the path the file gives, at the file's
own speeds; a fixture whose file ships no picture, or whose wheel is put in
on a channel of its own (a MegaPointe's), draws none. A fixture's own
programs (a Curve's tilt macros, a Spiider's patterns, a Nero's lightning)
are made up inside the fixture and no file says what they look like, so the
stage draws a stand-in: an effect program as the heads' level chasing along
the fixture, a move program as the head swaying in a figure of eight, both at
the program's rate, and PROGRAM STAND-IN at the bottom of the canvas names
the fixtures doing it. A moving head travels at the movement speed the
look sets, and an endless pan or tilt keeps turning. A brighter fixture,
by its file's lumens, reads brighter, and a narrow beam more so than a wide
one. A pixel fixture's heads draw their beams but no pools, and a raw channel
draws nothing.

**The native window.** It opens framed on your whole rig — however big the plot
is and wherever it sits — and stays where you put it after that; a patch edit
will not throw away the shot you were lining up. `1` `2` `3` are FOH, side and
top. `shift`+`4` to `9` keeps the view you are looking through in that slot, and
`4` to `9` goes back to it, so the drummer's eye line or the balcony is one key
away every time the window opens. Shots are kept per show, on this Mac, not in
the show file. `T` switches the tonemapper, the curve that decides how the
brightest beams roll off into white, so you can compare how a look reads under
each one; the window title names the one you are on. It looks through a normal lens rather than the wide one a game engine
defaults to, so the depth of the stage reads the way a photograph of it would;
`LIGHT_PREVIZ_FOV` sets the angle if you want a longer or wider one. A head
pointed at the camera flares, the way it does in the room. Moving heads are drawn as real moving heads: the yoke pans and the head
tilts with the beam, so you can see a fixture running out of travel or turning
to point at the audience, not just where the light lands.

Truss is drawn wherever the hang implies one — three or more fixtures in a line
at the same height and depth. It is a reading of your patch, not something the
show file carries, so a rig that is not hung in rows will not get bars drawn
through it.

**Risers.** Drag a musician onto a riser in the 2D plan and they stand on top of
it, kit and all — in both 3D views. There is no height to set: the stage reads
it off the riser your performer is standing inside, so moving or resizing the
riser moves whoever is on it. Only risers hold someone up, and standing beside
one leaves you on the deck.

**Exposure.** `auto exp` in the stage bar is eye adaptation: the view stops
down when the rig comes up and opens back up in the quiet parts, the way your
eyes do. It is partial, so a brighter look still reads brighter, and a blackout
is never brightened. Switch it off to judge absolute levels or to compare two
looks without the view re-metering between them.

**Layout.** The stage is a band across the **top** of every view — stages are
wider than they are tall, so that is the shape that reads. `Pads` / `Stage` /
`Rig` / `Build` (⌥1–⌥4) choose what sits under it:

| View | Under the band | |
|---|---|---|
| **Pads** | the pad grid, with the **look library** and the **look editor** at the right | the audition sits at the band's right edge |
| **Rig** | the fixtures table, with the **2D plan** above it | arriving picks the plan; your previous view comes back when you leave |
| **Build** | pads and the editor tabs | |
| **Stage** | — | full screen |

Drag the edges between panels to resize them, and the layout is remembered. Each
view has its own **hide** for the band (`▴` at the left of the stage bar, and
the slim strip it leaves behind brings it back), so you can run a show
full-height on the pads while the Rig view keeps its plan. **preview** in the
same bar switches the audition pane off — it is a second render, and firing a
pad selects it, so a show run from the pads may not want it.

**The look editor, on the pads view.** Rightmost column — the same editor as
the bottom panel's Look tab, following the same selection, so you can build the
next song without leaving the surface you perform from. `▸` folds it away. Below
about 810px of window width only one of the two right-hand panels fits, so
opening one folds the other; on a phone both fold and the pads keep the width.

**Look library.** Bottom right of the pads view: every look in the pool,
searchable, with a `×N` count of the pads already using it in this song. **Drag
a row onto any pad** to point that pad at the look. Pads *share* looks — editing
one updates every pad using it — so a drag is how the same wash reaches six
songs. `▸` collapses the library when the grid wants the width.

**Narrowing the library.** The chip row (`all · this song · on stage · flash ·
steps · unused`) ends in `filter`, which unfolds two more rows. `rig` is one chip
per group, in the order the GROUPS row draws them, then the kinds of light in
the rig — `moving heads · pars · bars · derbies · hazers` — and `does` is what a
look does to them: `wash · effects · strobe · moves · rainbow · modifier`. Press
any number: within a row it is *or* (derbies or pars), across rows it is *and*
(derbies, and strobes). A chip that matches no looks — counted on its own,
against the other rows, the chip above and the search — is greyed; a pressed
chip that now matches none reads `0` rather than quietly letting go; and
hovering any chip says how many it matches. The bar reads the pressed words
and the count (`derbies · strobe 12`), `clear` releases them all, and none of
it is a tag you maintain: which lights a look drives is read off its parts,
and what it does off its values and effects (a look that only opens the
shutter is not a strobe). The search box reads the same things, so `derb`
finds every look on the derbies and `ramp` every look with a ramp wave. The
same question runs the other way: `looks` beside a group (in the GROUPS head's
menu, or on the group's row in the Rig) and `looks that use this` on a light
on the plan open the library already narrowed to the looks that drive it. The
narrowing belongs to the show — a different show opens with none, and closing
the sheet on a phone drops it.

Dropping onto the pad a layer is **currently playing** does not change the
stage: the engine keeps playing the look it captured when you fired it. The pad
turns amber on screen to say so — fire it again to swap. On the APC that pad
keeps reporting the *stage*: it stays lit in the colour of what is actually
playing, not the colour of the look now sitting on it.

**The dial row.** Under the four layer rows, ruled off from them, sits a row
of **Dials**. They are not looks and not effects: a dial is one fader that
reaches *into* whatever is playing, each of its links driving one
parameter of one look's part between a `min` and a `max` you set. So it changes
nothing until the looks it links to are on stage, and it nudges live — the amber
**NUDGED** chip appears, and `Keep` writes the positions into the show while
`Discard` throws them away. A dial with no links yet is flagged ⚠.

The row is eight slots wide and the grid is capped at four layer rows for a
reason: four layers plus the dial row is exactly the 5 × 8 clip grid of an
APC40 mk2, so what is on screen is the shape of what is under your hands. Click
`+` on the next free slot to add a dial; `edit` opens the Controls tab, where
links, brackets and pulses live. Each fader is MIDI-learnable from the row
itself.

**On the network.** The engine serves this UI over HTTP too, and a device that
opens `http://<your-mac>:9900` without pairing can watch the show — the pads
animate and the status line reads WATCHING — but moves nothing. To drive the
show from the floor, pair the tablet: on the Mac open Settings ▸ Devices, press
*show pairing code*, and point the tablet's camera at it (or press *copy link*
and paste it on the tablet). Keep that link — a bookmark or a home-screen icon
stays paired until someone presses *unpair every device* on the Mac. On a phone
the look library folds itself away so the pads keep the width.

**Fixture library.** Rig view ▸ Fixture library is every fixture you can patch
from, in one searchable list: the built-ins, the generic layouts LIGHT ships
(pars with the dimmer and strobe in every common order, RGBA and RGBAW pars,
RGB and RGBW bars, a pixel strip — for any fixture whose channels are simply in
order; pick the mode that matches the manual), and every `.gdtf` you have
imported or fetched from GDTF Share on this Mac, which is why the next show
starts with them. When an update teaches the importer something new, the Rig
view flags profiles compiled by the older build with **rebuild from library**;
take the offer, and check the addresses after it, because a pixel array can
come back with more channels than it was first compiled with. `+ rig` patches one at the next free address; `✕` removes a
file from the library (fixtures already patched keep their profile — the
project carries it). The library lives with the LIGHT app; a browser or the
tablet sees the built-ins.

**Channel editor.** Rig view ▸ Profiles ▸ `channels` opens a profile and says,
channel by channel, what it sends. A built-in or an imported profile only
reads there. `copy to edit`, or `start from a built-in` under the table, puts a
copy in the show that you can change: the DMX channel each one is on, 16-bit
with its fine channel, the head it belongs to, what it rests at, and its cases.
A case is a condition and what to send while it holds: a fixed value, a level
that follows a parameter, the nearest colour slot, or a numbered slot. The
first case that holds wins, and with none the channel sends its rest value.
Anything that would send something you did not mean (two channels on one DMX
channel, a case that can never be reached) is listed above the table as you
go, and `move them to this copy` points the fixtures that used the original at
it. A profile made here is never rebuilt from the library, so an importer
update cannot overwrite your edit.

**Stage size.** The plan, the 3D views and the stage window fit themselves to
whatever is placed — a fixture or a piece of stage far out grows the floor to
hold it. Rig view ▸ Stage ▸ size sets it instead: width across, depth toward
the audience, height to the grid, in metres, centred on the plan's origin. A
set stage is drawn as typed, outlined on the plan with its size in the corner;
`auto` goes back to fitting.

**On a tablet.** The console notices a fingertip and switches to touch sizing:
every control is at least 24px, the pad's name strip is big enough to select a
look without firing it, and the song chips show their × and ‹ › all the time.
What a mouse reaches by right-click or hover, a finger reaches by **holding**:
hold a column head to rename, insert or delete it; hold a song to rename, move
or delete it; hold a musician or a piece of stage in the 2D plan to remove it.
In the plan a Move · Turn · Select picker stands in for ⌥ and ⇧. The **?** in
the top bar is help: tap it, then tap any control to read what it does (tap ?
again, or Escape, to stop). Settings ▸ Display forces touch sizing on or off —
for a laptop with a touchscreen, or a tablet with a trackpad.
