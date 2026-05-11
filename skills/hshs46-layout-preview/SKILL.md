---
name: hshs46-layout-preview
description: >-
  Updates the HSHS46 `layout-preview.html` SVG keyboard diagrams so they stay
  aligned with `hshs46-layouts.dtsi` geometry and with both keymaps (`boards/` module vs
  `config/boards/` user config). Use when the user edits layout-preview, keymaps, physical
  layout DTSI, mentions Studio/visual preview, or asks to sync layer legends with the keymap.
---

# HSHS46 layout preview (`layout-preview.html`)

## Goal

Keep **`layout-preview.html`** (repo root) accurate as a **human-readable** complement to **`hshs46-layouts.dtsi`**: same **46-key order**, positions **`x`/`y`**, pivot **`rx`/`ry`**, rotation (**centidegrees** /100 for SVG °). Show **all layers** for **both** keymaps in separate sections.

**Do not** add or rely on a codegen script for this repo—edit the HTML/JS directly.

## Source of truth

| What | Path |
|------|------|
| Geometry + key order | `config/boards/shields/hshs46/hshs46-layouts.dtsi` (`key_physical_attrs` lines). Mirror file: `boards/shields/hshs46/hshs46-layouts.dtsi` — keep identical geometry. |
| Module legends | `boards/shields/hshs46/hshs46.keymap` |
| Config legends | `config/boards/shields/hshs46/hshs46.keymap` |

If DTSI and keymaps disagree on **count** of positions vs bindings, the keymap is wrong for the matrix—fix the keymap or the transform, not the preview alone.

## HTML/JS structure (do not break)

1. **`geometryLabels(labels)`** builds each key row as `[x, y, rot, rx, ry, label, cls]`. **Coordinates and `cls`** come from the fixed template in `layout-preview.html`; **only the `label` strings** change per layer (via `MODULE_LAYERS` / `CONFIG_LAYERS`).

2. **`cls` column** (46 entries) encodes SVG draw order and pinky/col4 grouping:
   - `key-pinky56-L` / `key-pinky56`: outer columns drawn inside **±19°** rigid groups.
   - `key-col4-L` / `key-col4-R`: ring column; drawn inside **±15°** strip pivots `(303,184)` / `(1527,184)`.
   - `key-esc`: inner pair (orange styling).
   - `key-thumb`: thumb row.
   - `key`: everything else.

3. **`renderKeyboard`** draw order: non-special keys → left pinky group rotate → right pinky group rotate → col4 strips → inner esc pair.

4. **`SCALE`** (~0.30): shrink only for layout density; keep consistent across all cards.

## Updating after keymap changes

For **each** layer in each keymap:

1. Read the layer’s `bindings = < ... >;` block **only** (ignore `sensor-bindings` and ignore `//` diagram lines inside the matrix—they are not bindings).

2. Split into **46** binding tokens (same order as matrix positions top-to-bottom as listed in DTSI).

3. Update the matching **`labels`** array inside **`MODULE_LAYERS`** or **`CONFIG_LAYERS`** in `layout-preview.html`. Each inner array must have **exactly 46** short strings.

## Label conventions

- **`—`**: `&none`
- **`TR`**: `&trans`
- Prefer short ASCII: `GUI`, `SPC`, `BSPC`, `ENT`, `NAV`, `SYM`, `CAPWD`, arrows `↑↓←→`, etc.
- Pointing: e.g. `S←`/`S↑` for scroll, `M←`/`M↑` for move, `M1`/`M2` for mouse buttons—match the spirit of the user’s ASCII diagrams in the config keymap when present.

Tuning legend density is acceptable; accuracy of **which key does what** matters more than perfect typography.

## Updating after DTSI geometry changes

1. Copy **`x, y, rot, rx, ry`** from each `key_physical_attrs` line into **`geometryLabels`** (the numeric columns only—keep **`cls`** aligned **by index** with the same 46-key semantics).

2. Recompute **pinky rotation centres** if pinky key positions moved: bounding box of key centres for indices marked `key-pinky56` / `key-pinky56-L` (use key top-left + half `w`/`h` from DTSI if needed). Update **`PINKY56_CX`/`CY`** and **`PINKY56L_CX`/`CY`** in the script block.

3. If col4 pivot keys moved substantially, adjust **col4 strip** pivots in **`addCol4Strip(..., 303, 184)`** / **`1527, 184`** to match the middle-row ring **rx/ry** from DTSI.

4. Cross-check **`docs/hshs46-column-angles.md`** — if the doc promises sync with the preview, update the doc in the same change.

## Sections to preserve

- Intro paragraph explaining geometry vs legends and legend notation.
- Two **`buildSection(...)`** calls: module path vs config path; section titles should reflect actual filenames.

## Validation checklist

- [ ] 46 labels per layer; no duplicate/missing bindings vs keymap.
- [ ] Browser opens file; SVGs render without console errors.
- [ ] Module section matches **`boards/.../hshs46.keymap`** layers; config section matches **`config/boards/.../hshs46.keymap`** layers (same layer names / count as in source).
