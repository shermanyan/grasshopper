# Grasshopper

Parametric Grasshopper definitions for Rhino 8 that take a design from a few inputs all the way to parts a machine can
cut.

| Project | What it does |
|---|---|
| [**Concrete canoe mold generator**](canoe-mold/) | Designs a concrete canoe hull from three control curves, splits it into EPS foam mold blocks and writes 4-axis hotwire g-code for each block. |
| [**Parametric cabinet**](parametric-cabinet/) | Generates a complete plywood cabinet (carcass, drawers, doors, dados and hardware holes) as flat parts ready for a CNC router. |

[![Canoe mold generator](canoe-mold/images/canvas.png)](canoe-mold/)

[![Parametric cabinet](parametric-cabinet/images/cabinet-drawers.png)](parametric-cabinet/)

The canoe generator also has a web version: Will Rollins turned it into
[Canoe Studio](https://shopnerd.github.io/canoe-studio/), built with Claude.

Each folder has its own README with requirements, inputs and how each stage works.
