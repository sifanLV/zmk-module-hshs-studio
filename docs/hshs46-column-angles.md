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

**Right half (split side, inside → outside):** Where columns are columnar, **`x`** is constant per column. **Cols 1–2** (**YHN**, **UJM**) **`rot` 0**. **Middle finger column — E/D/C** (*mirrored*) **I/K/comma**: **`rot` 0** with **each key’s own **`(rx, ry)`** at its geometric centre** (e.g. **E**: **(458, 50)** through **comma**: **(1372, 239)**) so rotation does **not** introduce faux **row stagger** inside the column. **Ring column (col 3)** **O/L/DOT**: **`rot` +1500** (‑15° left **W/S/X** as **`−1500`**), **spine** **`x`** **1511 / 1477 / 1443** (**34** step toward split) plus vertical **`y`** **39 / 134 / 229**. **Cols 5–6** pinky **`rot` 0** in DTSI; **`layout-preview.html`** still draws pinky **`±19°`** as one group for a steeper envelope.

**`layout-preview.html` (ring strips):** **O/L/DOT** and **W/S/X** are drawn as axis‑aligned caps in **one** SVG **`rotate(±15°, pivot at middle row centre — S/L `rx,ry`)** per side (**303, 184** / **1527, 184**). **ZMK Studio** uses **per‑key** **`rot`** and **per‑key `rx,ry`** from DTSI (three separate ±15° tilts on the spine).

**Left half:** **W/S/X** spine **`x`** **219 / 253 / 287** (mirror of right ring). **E/D/C** same **`rot` 0 + own‑centre pivots** rule as the right middle column. Outer **`grave`…`Z`** **`x`** **58** / **163**. Preview pinky **`−19°`**. **Thumbs:** **+12** split gap vs earlier geometry.

**BBox note:** adjacent rows still use ~**95** vertical step between **`y`** values (**column stagger**); **100×100** boxes can show slight apparent overlap — stagger convention, not extra cap collision beyond thumb tuning.

### Column 0 (**ESC / SHIFT**, **CAPS / SPACE**)

Treat **ESC+SHIFT** (left) and **CAPS+SPACE** (right) as **one visual column each**: **`ESC`/`CAPS`** reuse the **same rotation pivot **`(rx, ry)`** as the thumb key directly below — **`SHIFT`**: **(735, 431)**, **`SPACE`**: **(1095, 431)** — with **`rot` ±2000** (‑20° / +20° mirrored) so the upper key does **not** “wander” sideways relative to the thumb under a different pivot.

### Right half — columns 1–6 (inside → outside)

| # | Keys | `rot` (cs) | Layout |
|---|------|------------|--------|
| 1 | Y, H, N | 0 | **`x` = 1122** all rows (columnar) |
| 2 | U, J, M | 0 | **`x` = 1222** |
| 3 | I, K, comma | **0** | **`x` = 1322**; **`rx` = 1372**; **`ry`** = row centre **50 / 144 / 239** |
| 4 | O, L, DOT | **±1500** (mirror left ring) | Spine **`x`**: **1511 / 1477 / 1443** (DOT); **`y`** **39/134/229** |
| 5 | P, SEMI, SLASH | **0** | **`x` = 1567**; preview: **group** +19° |
| 6 | BSPC, ENTER, SQT | **0** | **`x` = 1672**; preview: same group |

Thumb cluster: **left** thumb **`x/y/rot/rx/ry`** mirror **right** thumb (same **±10° / ±20° / ±30°** magnitudes, reflected **`rot`** sign and **`rx`**).

**ESC / CAPS:** **`rx` = 735** / **1095** (**same **`x`** as SHIFT / SPACE**); **`ry` = 431** (**same **`ry`** as those thumbs — shared pivot**, not a higher pseudo‑row **`ry`**).

---

## Photo vs **implemented** (sanity check)

| Photo envelope | Implemented | Notes |
|----------------|-------------|--------|
| Q **~15°–20°** CCW (` ` / TAB in photo) | **Mirror of right** (not a separate −20° / −17.5° / −11° stack) | Left **positions** track right **O/P/BSPC** geometry; photo can still read differently. |
| W **~10°–12°** CCW | **−15°** on **W/S/X** (`−1500` cs), mirrored from **O/L/DOT** | Ring shallower than **±19°** pinky block in preview. |
| Middle **≈ 0°** | **E/D/C** & mirror **I/K/,** **`rot` 0** (own‑centre pivots); **±15°** ring | Plate photo is approximate. |
| Inner index “tilt” in some photos | **Implemented** as **column stagger** (**`y`** per column) only, not **per-row `x`** | Columnar convention. |
| Right cols **5–6** vs **4** | Col **5–6** **`rot` 0** + rect; preview **+19°** group; col **4** **`+1500`** | Pinky steeper than ring (photo). |
| Left outer vs right | **Mirror** at **`x = 915`**; left rect **`rot` 0** + preview **−19°** group | Bilateral symmetry. |

---

## Column rotation differences: **historic note**

Earlier preview builds used **`−10° / −5° / −3⅓°`** on inner columns; **inner `x` stagger** for photo‑like tilt was added later in **`hshs46-layouts.dtsi`**.

---

## How to tighten the comparison (recommended)

1. **CAD / PCB** — measure switch‑cutout or cap‑pocket normals vs plate X/Y; paste into DTSI **`rot`** (cs).
2. **Photo overlay** — use a **straight reference** aligned to plate edge + protractor overlay on JPG; record Q / W / inner index columns separately.
3. **Sync** — when values change: `layout-preview.html`, `config/boards/shields/hshs46/hshs46-layouts.dtsi`, and `boards/shields/hshs46/hshs46-layouts.dtsi`.

---

## Revision

- **Col 3 ring** spine (**W/S/X**, **O/L/DOT**) with **−15° / +15°** per key and stepped **`x`** for colinear centres; **preview** one **SVG ±15°** strip per side (pivot at **S/L** row).
- **Col 4 (middle)** **E/D/C** & **I/K/comma**: **`rot` 0**, **each key’s own **`(rx, ry)`** — no shared middle‑row pivot (avoids row stagger in column).
- **Col 0** **ESC/CAPS**: same **`(rx, ry)`** pivot as **SHIFT** / **SPACE** (**`ry` 431**).
- **Cols 5–6** pinky: **`rot` 0** in DTSI; preview may still use **`±19°`** group. **`IMG_5455`** reference (**2026‑05**).
