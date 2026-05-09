# HSHS46 column angles: photo versus `layout-preview.html`

**Photo** visual read vs **implemented** rotations in **`layout-preview.html`** and **`hshs46-layouts.dtsi`** (aligned to the JPG — still see caveats below).

---

## Reference photo

**Versioned copy:** [`docs/IMG_5455.JPG`](IMG_5455.JPG)

The shot is roughly **top‑down** but **not** a calibrated orthographic view. Perspective and JPEG lens distortion can easily shift apparent tilt by **a few degrees**. Prefer **KiCad / plate STEP** angles when you want to match fabrication exactly.

**Convention below:** Photo angles are stated **relative to the image vertical** (“CCW” = counter‑clockwise from straight‑up on the JPG). Preview uses SVG `rotate()`, **degrees × 100** in the third tuple field (`rot_centideg`); pivot near key center **`(rx, ry)`**.

---

## Visual read (`docs/IMG_5455.JPG`) — left half plate

Interpretation aligned with image description “five finger columns left of thumb”; column index is **finger order from outside (pinky side) inward** on **this half only**.

| # | Role (physical) | Apparent tilt (photo) |
|---|----------------|------------------------|
| 1 | **Q column** | **≈ 15°–20° CCW**; three keys share one spine (no stair‑stepped sides). |
| 2 | **W column** | **≈ 10°–12° CCW**; aligned column; visibly **less** than column 1 but still strong. |
| 3–4 | **Middle** (e.g. E/D … / R/F …) | **≈ 0°** (sides roughly parallel to image vertical). |
| 5 | **Inner index (“T / G” side)** | **≈ 10°–15° CW** relative to vertical; outward lean **toward the hand center / split**. |

Thumb cluster is an **arc** (per‑key rotation varies; omitted from the table below).

---

## Implemented — `layout-preview.html` / `hshs46-layouts.dtsi`

**Right half (split side, inside → outside = columns 1–6):** cols **1–3** at **`rot` 0**. **Col 4** (**O / L / DOT**) uses **`rot` +1500** (**+15°**). Key **centers** are **exactly colinear**: **O** **(1561, 89)**, **L** **(1527, 184)**, **DOT** **(1493, 279)** — constant step **(−34, +95)** between consecutive centers (top-left **`y`** **39 / 134 / 229** for **DOT**). Pivots **`rx`/`ry`** at key centers. **Cols 5–6** **`x`** **1567** / **1672** (**−55** vs prior **1622** / **1727** so bottom gap toward col 4 matches thumb‑row‑2 spacing better), **`rot` 0**.

**`layout-preview.html` (column 4 only):** **O/L/DOT** and **W/S/X** are drawn as **axis-aligned** caps inside **one** SVG **`rotate(±15°, pivot at L/S center)`** each, so the column looks like a **straight strip**. **ZMK Studio** still uses per-key **`rot`** from DTSI (three separate +15° / −15° tilts). Pinky blocks **`±19°`**; bbox **`x` ∈ [1567, 1772]**, **`y` ∈ [176, 462]**, group pivot center **≈ 1669.5, 319**.

**Left half:** mirror at **`x = 915`**; **W / S / X** centers **(269, 89)**, **(303, 184)**, **(337, 279)** (step **(+34, +95)**). Outer **`grave`…`Z`** **`x`** **58** / **163**. Preview **`−19°`**. **Thumbs:** **±12** split gap.

**BBox note:** adjacent rows in the same column still use **~95** vertical offset between **`y`** values (column stagger); axis‑aligned **100×100** boxes therefore show **~5** unit “overlap” — that is the stagger convention, not extra cap collision beyond the thumb fix above.

### Right half — columns 1–6 (inside → outside)

| # | Keys | `rot` (cs) | Layout |
|---|------|------------|--------|
| 1 | Y, H, N | 0 | Tight with 2–3 (`x` **1122**) |
| 2 | U, J, M | 0 | `x` **1222** |
| 3 | I, K, comma | 0 | `x` **1322** |
| 4 | O, L, DOT | **+1500** | Centers colinear step **(−34, +95)**; top-left **`y`** **39/134/229** |
| 5 | P, SEMI, SLASH | **0** | **`x` = 1567**; preview: **group** +19° |
| 6 | BSPC, ENTER, SQT | **0** | **`x` = 1672**; preview: same group |

Thumb cluster: **left** thumb **`x/y/rot/rx/ry`** mirror **right** thumb (same **±10° / ±20° / ±30°** magnitudes, reflected **`rot`** sign and **`rx`**).

---

## Photo vs **implemented** (sanity check)

| Photo envelope | Implemented | Notes |
|----------------|-------------|--------|
| Q **~15°–20°** CCW (` ` / TAB in photo) | **Mirror of right** (not a separate −20° / −17.5° / −11° stack) | Left **positions** track right **O/P/BSPC** geometry; photo can still read differently. |
| W **~10°–12°** CCW | **−15°** on **W/S/X** (`−1500` cs), mirrored from **O/L/DOT** | Ring shallower than **±19°** pinky block in preview. |
| Middle **≈ 0°** | **0°** on **E–T** block | Match. |
| Inner index (**T**/ **G**) **~10°–15° CW** | **0°** (**T,G,B**) | **Vertical** inner block. |
| Right cols **5–6** vs **4** | Col **5–6** **`rot` 0** + rect; preview **+19°** group; col **4** **`+1500`** | Pinky steeper than ring (photo). |
| Left outer vs right | **Mirror** at **`x = 915`**; left rect **`rot` 0** + preview **−19°** group | Bilateral symmetry. |

---

## Column rotation differences: **historic note**

Earlier preview builds used **`−10° / −5° / −3⅓°`** and **no **`T`/ `G`/ `B`** tilt**, which diverged sharply from this photo. Those rows are superseded by the **Implemented** table above.

---

## How to tighten the comparison (recommended)

1. **CAD / PCB** — measure switch‑cutout or cap‑pocket normals vs plate X/Y; paste into DTSI **`rot`** (cs).
2. **Photo overlay** — use a **straight reference** aligned to plate edge + protractor overlay on JPG; record Q / W / inner index columns separately.
3. **Sync** — when values change: `layout-preview.html`, `config/boards/shields/hshs46/hshs46-layouts.dtsi`, and `boards/shields/hshs46/hshs46-layouts.dtsi`.

---

## Revision

- **Col 4** DTSI colinear centers + per-key **±15°**; **preview** one **SVG rotate** per side for straight strip. **`IMG_5455`** (**2026‑05**).
- **Cols 5–6** (and mirrored outer pinky): **`y` / `ry`** **+50** (half key height, **h = 100**) along the column.
