# Realm of the Elderlings — an Atlas

A cartographic companion to Robin Hobb's *Realm of the Elderlings* novels: an
iPhone app that draws the world as a single hand-engraved sheet, redrawn from
geometry every time it opens rather than shipped as a picture.

**Platform** iPhone, via [Scriptable](https://scriptable.app)
**Author** Personal project. No commercial intent, no client, no deadline
beyond my own patience.

## What's here

Two files, and they work together:

- **`Atlas.js`** — the atlas itself. A Scriptable script that generates the
  map — coastlines, relief, woodland, rivers, shore hatching, paper texture
  and vignette — from control geometry each time it runs, and shows it in a
  pinch-and-pan web view. The same input always produces the same output;
  nothing here is a bitmap.
- **`Atlas-Tracer.html`** — the editing tool. A self-contained browser app
  for tracing that control geometry off the published Robin Hobb map plates
  and correcting it by hand: drag points, draw new coastline, plant woods,
  name places and rivers, and export the result as `atlas-geo.json`, which
  is what `Atlas.js` draws from.

There's no build step and no dependencies — both files run as-is.

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

Work is autosaved to the browser's local storage as you go. **Save** writes
`atlas-geo.json`; **Import** reads one back in, replacing the current
geography (and the plate placements that go with it).

Full in-app documentation — every tool, every field, every keyboard
shortcut — is under the **?** sheet in the tracer itself.

## Status

Phase 1, the sheet, is essentially done: the map currently carries 60 coast
outlines (5,616 control points), 23 rivers, 517 trees, 11 relief regions and
44 settlements, all traced from the published plates. Naming — turning that
geometry into a labeled, searchable gazetteer — is underway and is the
harder problem.

Design notes worth knowing if you're reading the code: no territory is ever
tinted (fills would spill into the sea and muddy the paper), so each realm
is told apart only by the ink on its settlements' rings and region names —
nine hues solved for contrast against the paper and against each other.
Land and sea are similarly separated by luminance, not just hue, so the
coastline still reads in poor light.
