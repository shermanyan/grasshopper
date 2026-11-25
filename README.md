# Concrete Canoe Mold Generator

A parametric Grasshopper definition that designs a concrete canoe hull from three control curves and turns it into a
stack of EPS foam mold blocks, ready to cut on a 4-axis CNC hotwire cutter. Built for the USC ASCE Concrete Canoe team.

**Web version:** Will Rollins turned this script into a web-based version, built with Claude:
[Canoe Studio](https://shopnerd.github.io/canoe-studio/). Same idea in the browser: drag the curves, see the hull and the
mold blocks update, and export.

![Canoe Studio](images/canoe-studio.png)

## Requirements

- Rhino 8 with Grasshopper (the file was saved in Grasshopper 8.27)
- [Pufferfish](https://www.food4rhino.com/en/app/pufferfish) plugin (used for the tween curves)

Open Rhino, run `Grasshopper`, then open `canoe_script_012026.gh`. Everything is driven by sliders and curve controls on
the left of the canvas; the previews show up in the Rhino viewport.

## How it works

The definition runs left to right in four stages: **shape the hull → build the skin → place it in the foam → cut the
blocks.**

### 1. Shape the hull with three curves

A canoe hull is defined by three curves, each with its own group of controls:

| Curve | What it sets | Controls |
|---|---|---|
| **Top curve** | The gunwale seen from above: how the beam narrows from midship to the bow | `top curve controls` |
| **Forward curve** | The profile seen from the side: keel, stem and how the bow rises | `forward curve controls`, `rocker control`, `rocker offset` |
| **Cross section** | The midship section: flat bottom, bilge turn and sides | `cross section curve controls`, `bottom setback`, `midpoint setback X / Y / Z` |

`bottom setback` sets how far the flat of the bottom stops in from the side, and the midpoint setbacks move the bilge
control point. The `flat back setting` group squares off the stern end, and `canoe smoothness control` sets how many
sections the hull is built from.

### 2. Build the skin

The cross section is tweened toward the forward curve (Pufferfish **Tween Curve**), giving a family of section curves
from midship to the bow. Each section is then rotated, dropped to the keel line (`position tweens to bottom`) and scaled
so its top lands exactly on the top curve (`scale to match top curve`, `extend tween to top curve`).

The sections are lofted into one surface, mirrored to make the full hull, capped and joined (`solidify canoe`). Volume
and area of the closed hull are computed for checking weight and buoyancy.

### 3. Place the canoe in the foam

`canoe placement` positions the hull inside the foam stock with offsets from the block bottom (`canoe x offset`,
`canoe Z offset`) and a `top trim plane` that sets where the mold is cut off at the gunwale.

### 4. Cut it into mold blocks

`make blocks` lays out a grid of foam blocks from `block sizes`, `num rows` and `num blocks per row` (a rectangular
array), drops rows that are not needed (`remove rows`), and subtracts the canoe from the foam (`trim blocks`, a solid
difference). What is left is the male mold, split into blocks the size of the foam you actually have.

Previews let you check each stage before cutting: `canoe skin preview`, `canoe contours preview`,
`canoe guide contours preview`, `mold blocks preview` and `drawing validation preview`.

## From script to canoe

Each block's two faces are different hull sections, which is why the cutter needs four axes: one tower at each end of
the wire, each moving on its own, so the wire sweeps the surface between the two profiles.

![4-axis hotwire cutter](images/hotwire-cutter.jpg)
![Toolpaths in Rhino](images/toolpaths-in-rhino.jpg)

A small test mold came first, then the full-size blocks.

![Test mold](images/test-mold.jpg)
![Mold fit-up](images/mold-fit-up.jpg)
![Full mold](images/mold-full.jpg)
![Finished canoe](images/finished-canoe.jpg)

This file covers the hull and the mold blocks. The g-code for the cutter is not part of this definition.

Written up by USC Viterbi:
[How to design a competitive concrete canoe](https://viterbischool.usc.edu/news/2025/05/how-to-design-a-competitive-concrete-canoe/).

## Files

- `canoe_script_012026.gh`: the Grasshopper definition
- `images/`: canvas, web version and build photos
