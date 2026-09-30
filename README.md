# Mod Tree Visualizer (Vertical)

A single-file, dependency-free prototype for visualizing a large mod/tech
tree by cost. Instead of a standard logarithmic axis, magnitude is laid out
so every decade (1x-10x) gets equal vertical space, and every step within a
decade gets equal space too — so cheap, early tiers stay just as readable as
astronomically expensive ones further up the tree.

Built as a quick mockup to explore layout ideas before committing to a full
implementation.

## Inspiration

This project started as an exploration of the mod/tech tree from *Cell Idle
Factory Incremental*, aiming for a layout that could scale to the same kind
of very-high-magnitude cost progression while staying navigable. It's an
independent fan project — not affiliated with, endorsed by, or built using
assets from that game.

## Features

- **Magnitude-aware vertical scale** — position is `exponent + (mantissa-1)/9`,
  so 1->2 within any decade takes the same vertical space as 3->4 within any
  other decade, and every full decade takes the same total space.
- **Per-mod columns** that reflow and condense — a mod's column only appears
  while at least one of its levels is inside the current scroll viewport;
  otherwise it's dropped entirely and the remaining visible columns spread
  out to fill the freed space.
- **Buy-to-unlock progression** — each mod shows one "plaque" (its current
  purchasable level); buying it reveals the next level up the chain. Levels
  gated behind an unbought prerequisite mod render dimmed and are unclickable.
- **Magnitude-boundary previews** — future levels that land exactly on a
  `1eN` boundary render as small preview plaques even before they're
  reachable, so you can see roughly how far a mod's chain goes.
- **Circle thinning** — mods with more than 30 levels only render 1 in 10 of
  their non-plaque levels as small circles, to keep very long chains legible.
  Toggle-able.
- **CSV-driven** — loads directly from `mods.csv` / `connectors.csv` via the
  browser's file picker (nothing is uploaded anywhere; it's all read
  client-side). A synthetic dataset generator is built in for testing without
  real data.
- **No build step, no backend, no dependencies** — it's one `.html` file.
  Open it in a browser and it runs.

## Getting started

1. Open `mod_tree_vertical.html` in a browser (double-click, or drag it into
   a tab).
2. Either:
   - Click **Load synthetic data** to try it immediately with generated
     placeholder data, or
   - Use the **mods.csv** / **connectors.csv** file pickers to load your own
     data (see format below). Both files need to be loaded before the chart
     renders.
3. Scroll vertically to move through magnitudes. Columns will appear and
   disappear as their levels enter and leave the visible range.
4. Click a mod's current plaque to buy that level.

## Data format

**`mods.csv`** — one row per level, per mod:

| Column     | Meaning                                                              |
|------------|-----------------------------------------------------------------------|
| `Code`     | Unique mod identifier. Repeated across all of that mod's level rows.  |
| `mod_name` | Display name.                                                         |
| `mod_level`| Level number (integer).                                               |
| `mod_type` | Used for the fallback color when no image is given.                   |
| `x`        | Sort hint for left-to-right column ordering.                          |
| `cost`     | The level's cost. See supported formats below.                        |
| `image`    | Optional image URL/path for the plaque. Falls back to a colored box.  |

Supported `cost` formats:
- Plain numbers: `1500`, `2000000`
- Scientific notation: `1e40`, `2.5E+21`
- Suffix shorthand: `500k`, `2.5m`, `10qa` — supports
  `k, m, b, t, qa, qu, sx, sp, o, n, d` (thousand through decillion)

**`connectors.csv`** — one row per prerequisite edge:

| Column        | Meaning                                      |
|---------------|-----------------------------------------------|
| `Code`        | The mod being unlocked (the child).           |
| `Unlocked By` | The mod code that must be bought first (the parent). |

A mod with no row in `connectors.csv` is treated as a root and is buyable
from the start.

## Known limitations

This is a mockup, not a production tool:
- All state (what's bought) lives in memory only — refreshing the page
  resets everything. There's no save/load.
- No build tooling, tests, or bundling — it's intentionally a single file.
- Layout and interaction details are still being iterated on; expect rough
  edges.

## License

See [LICENSE](LICENSE).
