# The Realm of the Elderlings — an Atlas

A cartographic companion to Robin Hobb's *Realm of the Elderlings* novels,
built for the iPhone as a personal, no-deadline project.

**Platform** iPhone, via [Scriptable](https://scriptable.app)
**Author** Personal project — no commercial intent, no client, no deadline
beyond my own patience.
**Audience** One reader, partway through the books, reading in order,
unspoiled, using it in bed on a small screen. No onboarding, no second
audience.

## What this is

Sixteen novels take place across a world readers are expected to hold in
their heads — a journey of three weeks, a duchy said to bleed first, a river
that eats wood and flesh. The published maps answer some of this, but
they're fixed, printed objects with no way to ask them a question.

This is a map you *can* ask questions of: how far Jhaampe is from Buckkeep,
which duchy holds Forge, what the Rain Wild River connects to, why the Red
Ships reach Bearns first. It should open in a second on a phone, and be
beautiful enough to open even with nothing to look up.

## The central idea

**The map is drawn, not photographed.** Rather than a pan-and-zoom viewer
over a scanned map image, every coastline, mountain, tree and wave mark is
generated from a small set of control geometry each time the atlas opens. A
coast is a few dozen points and a rule for making them wander; a mountain
range is traced ground and a rule for standing peaks on it; a wood is the
symbols the engraver drew, resolved into a stand of trees as you close in.

This buys four things: crispness at any zoom (nothing is an enlarged
pixel), intelligence at every scale (what's drawn can differ by how close
you are), correctability (geography is numbers with meaning, not a bitmap),
and character (controlled randomness gives coastlines the irregularity of a
hand).

It must still be **identical every time it opens** — randomness is a
drawing technique, not a feature. And a rule that fell out of hard
experience: **a scattered layer must grow denser without moving.** Anything
scattered across the sheet (wave marks, stipple, trees) sits on a hierarchy
of ranks, each twice the spacing of the one below; a rank's marks never
move once laid, closing in only adds a finer rank fading up from nothing.

## Design principles (ordered — earlier wins where they conflict)

1. **Design quality is a hard constraint**, not a dimension to trade against
   features.
2. **Hierarchy through weight, colour and scale — not size.** Type sizes
   span a narrow range; importance is carried by weight, colour and whether
   something appears at all.
3. **Automatic over manual.** No declutter slider, no manual label controls,
   no zoom buttons.
4. **Density with elegance, not simplicity through omission.** The widest
   view is spare because that's legible at that scale — not because the
   atlas is thin.
5. **Named things over icons.** Interface controls say what they are.
6. **When it fails, say so.** A half-drawn map with no explanation is worse
   than an honest error.

## Visual direction

**Voice:** ink on laid paper, in the tradition of an endpaper map in a
hardback — precise, quiet, dense, warm, drawn with restraint (no
faux-aging, no dragons in the corner). The sea is treated as a material,
not a background: it carries a drawn surface everywhere (short shallow
arcs, no two alike) because more than half of nearly every portrait view is
water.

**Palette:** warm paper, grey-green sea, brown-black ink, with **one
reserved accent** — a red oxide (`#a33c26`), held for selection, personal
marks and journey routes only. No territory is ever tinted (fills would
spill into the sea and muddy the paper); instead each of the nine realms
gets its own ink, applied to settlement rings and nothing else — the
*only* thing answering "whose is this?" No name is coloured for its realm:
a seat is told by weight and a filled ring beneath it, and a region name is
set in the secondary ink, a step back from a settlement's, so it carries
the ground rather than competing with the places on it. Colour on the sheet
answers one question, quietly, for a reader who thinks to ask it. The nine
inks are solved for contrast (4.6:1–7.2:1 against paper) and spaced at
least 22 Lab units apart, rather than chosen by eye. Land and sea are
likewise separated by luminance, not just hue, so the coastline still reads
in poor light.

**Typography:** two voices, strictly separated — Baskerville for content
(labels, articles, notes), system sans for chrome (tabs, buttons, scale
bar). Settlement names roman, water names italic in water ink, region names
small-capped in the secondary ink, river names set *along* the river with
each letter turned to the line's direction. Sizes span 10.4–13.6 points —
under a third of an octave. Tracking is a property of the type and lives in
the type spec: a region name is set at 0.13 em and a water name at 0.08,
close enough that the word still reads as a word. Which letters are small
caps is decided by where they fall in the name, never by how the name
happens to be stored, so two names on one view are never set in two
different styles. Labels carry a soft halo the colour of the ground
beneath them, never a box.

**Symbols:** the three tree species are one family — shared baseline, bole,
crown construction and proportion — not three unrelated drawings at the
same size. Which species, whether it's scrub, and when it appears are all
*decisions* (smooth fields, isolation, blue-noise ordering), never dice.

**Motion:** the atlas is a reference object and should be still — but
stillness isn't stiffness. Things gated by continuous zoom (a mountain
resolving, a wave rank) need no clock, since they're already functions of
the gesture. Things gated by discrete zoom levels (names, marks, rivers)
get one shared timed fade. Only three moments are allowed to be felt: the
zoom's give at its limits, a name sliding to a new position, and the scale
bar's numbers crossing over. Everything else is invisible. Every duration
collapses to zero under `prefers-reduced-motion`.

## What the atlas contains (planned, in full)

- **Geography** — the full arrangement of the world faithful to the
  published maps: the Six Duchies (Bearns, Buck, Rippon, Shoaks, Farrow,
  Tilth), Buck Bay and Buckkeep, the Mountain Kingdom, the Rain Wilds,
  Chalced, Bingtown and Trader Bay, the Out Islands, the Glacier Plains, the
  Pirate Isles, Jamaillia and Clerres.
- **Writing** — a short article for every place, in the atlas's own dry,
  specific voice, with cross-references. Realms get longer essays.
- **Find** — search the gazetteer by name or article text, grouped by realm.
- **Journeys** — half a dozen curated routes through the books, stepped
  waypoint by waypoint with prose.
- **Layers** — independent toggles for settlements, names, borders, relief,
  woodland, rivers, roads, sea lanes, conjectural links.
- **Reading position** — set once (per book, 16 steps), then invisible —
  gates the map, gazetteer, journeys and cross-references so nothing spoils
  ahead of where the reader has read.
- **Marks** — save places, mark as read, keep notes.
- **Measure** — lay a course and get leagues/miles, with days by ship,
  horse, foot and dragon.
- **A widget** — a place-a-day on the home screen, in the atlas's own paper
  and type.

**Constraints:** one file (the whole atlas lives in a single droppable
script, no build step), offline after first launch, portrait phone,
one-handed, thumb reach only, nothing leaves the phone (no accounts, sync
or analytics), fast enough to feel instant.

**Out of scope:** claims of canonicity, newcomer onboarding,
sharing/export/sync, character databases or plot summaries, and any content
the reader couldn't have reached at their stated point in the series.

## Platform reality (Scriptable)

The map has to live inside a full-screen `WebView` running a canvas —
Scriptable's native drawing can't pinch, pan or hit-test. State comes back
out of the page via a persistent save channel (the script awaits
callback-form evaluations the page completes when it has something to
persist) plus a close-out read on dismissal. Baskerville, offline-from-first-
launch, and pixel-crisp canvas rendering all come for free on iOS. The one
real platform limit: **widgets can't use a WebView**, so the home-screen
widget will be drawn separately with `DrawContext`, behind the same thin
drawing-context interface as the map.

## What's here

- **`Atlas.js`** — the atlas itself. A Scriptable script that generates the
  map — coastlines, relief, woodland, rivers, shore hatching, paper texture
  and vignette — from control geometry each time it runs, and shows it in a
  pinch-and-pan web view. The same input always produces the same output;
  nothing here is a bitmap.
- **`Atlas-Tracer.html`** — the geography workbench. A self-contained
  browser tool for tracing that control geometry off the published Robin
  Hobb map plates and correcting it by hand: drag points, draw new
  coastline, plant woods, name places and rivers, and export the result as
  `atlas-geo.json`, which is what `Atlas.js` draws from.
- **`atlas-geo.json`** — the geography itself, as the tracer last exported
  it. It is the source the other two are regenerated from: `Atlas.js`
  carries it as its `PLATE`/`COASTS`/`WOODS`/… constants, and the tracer
  carries it as the `GEO0` it starts a fresh session from. All three are
  updated together, so a copy is never quietly older than the map.

There's no build step and no dependencies — both files run as-is.

The loop: open the tracer → fix geography (trace a coast, name a river) →
**Save** downloads a fresh `atlas-geo.json` → that file gets regenerated
into `Atlas.js` → drop the new `Atlas.js` into Scriptable. The tracer
autosaves your in-progress work to the browser's own storage between
sessions, so nothing is lost by closing the tab — pressing Save is only for
handing a copy off, not for keeping your place.

### Where the geography comes from

Ten reference images — two full Six Duchies plates, four nautical charts
(Cursed Shores, Hawser Channel, Trader Bay, the Wostian Sea), and a
Withywoods floor plan (open question — likely left out). Coastlines are
traced by machine (thresholded, contour-extracted, simplified), not
eyeballed — an early lesson was that hand-read coordinates from a plate
were off by up to 5% of its width. Where plates disagree on geography
rather than just scale, **the sheet whose centre is nearer wins**. Where
the plates are silent (the far south especially), placement follows what
the books imply, and a `conjectural` flag marks it — drawn with a broken
ring rather than solid.

All geography lives in one file, `atlas-geo.json`, edited with the tracer
and regenerated into `Atlas.js`. Nothing in the atlas is a hand-typed
coordinate.

## Running the atlas

1. Install [Scriptable](https://scriptable.app) on your iPhone.
2. Copy `Atlas.js` into Scriptable's script folder (iCloud Drive →
   Scriptable, or share it into the app directly).
3. Run the script. It opens the map in a web view — pinch to zoom, drag to
   pan.

If something fails to draw, the script says so on screen rather than
showing a half-finished map.

## Editing the geography

Open `Atlas-Tracer.html` in a desktop browser — it needs a mouse or
trackpad, not a touchscreen. It's one file with no server required; just
open it directly.

The tracer works against two backdrops you can cross-fade between: the
published plate you're tracing *from*, and the atlas's own rendering of the
current geometry, so you can see whether an edit actually helped. It shares
its drawing and label-placement code with `Atlas.js` itself, so what you see
while editing is what the phone will draw.

The geography is organized into eight layers — coast, rivers, ice, relief,
woods, places, names, lakes — edited one at a time with tools for drawing,
adding, moving, attaching, detaching and smoothing points. A built-in
**Still to do** panel walks the whole map looking for things that need
attention: unnamed rivers, points sitting in open water, duplicate names,
outlines that never closed.

Full in-app documentation — every tool, every field, every keyboard
shortcut — is under the **?** sheet in the tracer itself.

## Build plan

| Phase | Contents | Status |
|---|---|---|
| 0 | The spike — proves WebView, canvas, Baskerville, save channel and close-out read work on-device | Delivered |
| 1 | The sheet — the map drawn, no names, no interface | Delivered |
| 2 | The names — full label engine, placement, curved labels, LOD | Delivered |
| 2a | Geography re-traced by machine instead of by eye | Delivered |
| 3 | The gazetteer and the writing — articles, search, cross-references | **Next** |
| 4 | The reading position — one predicate gating map, gazetteer, search, journeys | Not started |
| 5 | Journeys, measure, marks, layers, legend | Not started |
| 6 | The widget, and hardening (atomic writes, error handling) | Not started |

Each phase is required to open on the phone at the end of it — nothing
ships half-runnable.

## Current status (as of 1 September 2026)

**Delivered:** the drawn, lettered sheet — six named materials (paper,
ground, canopy, sea, wave/shore/rock, ink/water), four levels of detail
plus a fifth "never drawn" (searchable-only) tier, 126 named things placed
with zero label overlaps at any of the thirteen zoom buckets — and the
level gates now hold most of them back, so the world view carries the six
realm names and nothing else. A lake is drawn wherever its own gate says,
but its name waits for the closer views: the water is geography, the word
is not. The underlying control geometry currently
carries 60 coast outlines (5,616 control points), 24 rivers, 5 lakes,
1,620 trees, 15 relief regions and 63 settlements, all traced from the
published plates.

**Not yet in the world:** the Out Islands and Pirate Isles have no
coastline yet (structural, not a naming gap — the Out Islands do now at
least carry their region name); Clerres is absent entirely; a handful of
Six Duchies settlements (Hook, Besham, Antler, Watch, Egg, Rook) need
hand-authoring; the west coast below the Rain Wilds is an honest
placeholder closure, not a traced shore, because no plate charts it.

**Still unread on the plates:** nine of the sixty-three settlements are
carried unnamed (six towers, two towns, a seat), six rivers are traced but
unnamed, one lake is unnamed, and three separate reaches all answer to
"Sanger River", so the name is lettered on each. All of it is visible in
the tracer's **Still to do** panel, which is where it gets fixed.

**Not yet built:** roads, borders and sea lanes are specified but undrawn.
Nothing is tappable yet — articles, the gazetteer and the reading position
are Phase 3, next up.

**A running theme of the build log:** several rounds of "finished" work
were later found to be visually broken in ways that didn't show up in code
review — shore hatching running inland, mountains being deleted under
labels, canopy bleeding into the sea, realm colour appearing on settlement
names contrary to the brief. The recurring lesson: *"it renders" and "it is
drawn" are different claims* — everything gets checked by looking at
rendered screenshots at all zoom levels, not just by reading the code.

## Open questions (not blocking)

1. **Scale calibration** — which stated in-book journey should fix the
   sheet's scale (currently a provisional 2 miles per world unit, from
   Buckkeep–Jhaampe).
2. **Travel speeds** for measure (ship / horse / foot / dragon) — to be
   proposed from the books' own timings.
3. **The six journeys** — routes sketched in the brief, waypoints still to
   be named.
4. **Withywoods** — whether the floor-plan reference belongs in an atlas of
   places at all; leaning toward leaving it out for now.
5. **Type ramp size** — whether 10.4–13.6pt reads well enough at arm's
   length in bed with one lamp on; only testable on-device.
