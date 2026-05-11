# zmk-keyboards-hshs

ZMK keyboard support for the **HSHS46** split keyboard by [beekeeb](https://beekeeb.com).

**Scope:** This repository covers **HSHS46 only**. There is no **HSHS52** board or firmware here, because HSHS52 is omitted until there is hardware available to test it.

This repository serves dual purpose:

- A **ZMK keyboard module** — Zephyr module metadata is in **`zephyr/module.yml`** (`board_root` points at this repo). **Module-compatible shields live under `boards/`**, especially **`boards/shields/hshs46/`**. Add this repository with `zmk module add`, not Beekeeb’s example config repo (see below).
- A **ZMK user config** — use the `config/` tree with West for fast firmware builds

Beekeeb publishes **[beekeeb/zmk-config-hshs](https://github.com/beekeeb/zmk-config-hshs)** as an **example ZMK user configuration** (West + `config/`). It is **not** packaged as a Zephyr/ZMK keyboard module (no **`zephyr/module.yml`** wiring a `board_root` into the module system), so **`zmk module add https://github.com/beekeeb/zmk-config-hshs` does not apply**. Use **this** repository for `zmk module add`; use Beekeeb’s repo as a reference for a standalone config layout (boards typically live under its `config/` tree, not as an installable module).

## Repository layout

- **`boards/`** — **Module-compatible** shield definitions (what ZMK picks up after you `zmk module add` this repository). The keymap here is kept **aligned with upstream** for module validation in the future.

- **`config/`** — Active **West user config** (`config/west.yml`) for **quick local firmware builds**. Shield sources and the keymap the build uses live under `config/boards/shields/hshs46/`.

- **Custom keymaps** — Edit **`config/boards/shields/hshs46/hshs46.keymap`** directly; that is the file West/ZMK uses when you build from this repo’s `config/` tree. If you prefer to keep a scratch copy or alternate layout elsewhere, you can maintain another file under **`config/<your-folder>/`** and copy or symlink it over that path before building.

CI firmware builds use `build.yaml` at the repository root.

## Layout preview and agent skill

- **`layout-preview.html`** (repo root) — static SVG overview of the **physical layout** and **short legends per layer** for both keymaps (`boards/...` module vs `config/...` user config). Open it in a browser after editing keymaps or geometry; it does not ship in firmware.

- **`skills/hshs46-layout-preview/SKILL.md`** — instructions for AI assistants on how to keep **`layout-preview.html`** in sync with **`hshs46-layouts.dtsi`** and the two **`hshs46.keymap`** files (46 bindings per layer, SVG structure, label conventions).

### Using the skill from common setups (humans)

Skills are not magic by themselves: your editor or CLI has to **load** `SKILL.md` into context (or you paste it). Typical options:

| Harness | What to do |
|--------|------------|
| **Cursor** | **Rules:** add a project rule under `.cursor/rules/` that tells the agent to follow `skills/hshs46-layout-preview/SKILL.md` when editing `layout-preview.html`, keymaps, or `hshs46-layouts.dtsi`. **Or** symlink `skills/hshs46-layout-preview` into `~/.cursor/skills/` so it appears among Cursor skills (same layout: one folder per skill containing `SKILL.md`). **Or** `@`-mention `skills/hshs46-layout-preview/SKILL.md` in Composer/chat when requesting preview updates. |
| **GitHub Copilot** (VS Code) | **Instructions:** in VS Code Copilot settings or repo **Workspace Instructions**, add a line pointing agents at `skills/hshs46-layout-preview/SKILL.md` whenever changing keymaps or `hshs46-layouts.dtsi`. Alternatively paste relevant sections into chat per task. |
| **Claude Code / `claude` CLI** | Add a short pointer in **`CLAUDE.md`** at the repo root (e.g. “When updating `layout-preview.html`, read `skills/hshs46-layout-preview/SKILL.md`”). Or run `/memory` / project docs per vendor docs and include that path. |
| **Windsurf / Codeium** | Add a note in **`.windsurfrules`** or Cascade rules referencing `skills/hshs46-layout-preview/SKILL.md` for layout-preview maintenance. |
| **Continue** | In `.continue/config.json` (or your rules block), reference the skill path or paste the checklist into **custom instructions** for this workspace. |
| **Any chat agent** | Attach **`skills/hshs46-layout-preview/SKILL.md`** or paste its **Validation checklist** and **Source of truth** table into the prompt when asking to sync the preview. |

If your tool only supports a single “project instructions” file, copy the **Goal**, **Source of truth**, and **Validation checklist** sections from `SKILL.md` into that file—or keep one short paragraph there that points to `skills/hshs46-layout-preview/SKILL.md`.

## Using as a Module (Recommended)

From your ZMK **user config** repository (the project that contains your `config/west.yml` and build):

```sh
zmk module add https://github.com/<your-fork>/zmk-module-hshs-studio.git
```

Replace `<your-fork>` with the GitHub path where this repository lives (for example your fork of this project). That wires in **`zephyr/module.yml`** so **`boards/shields/hshs46/`** is on the board search path.

Then run `zmk keyboard add` and select **HSHS46** (shields are defined under **`boards/`** in this module).

For a standalone example **without** a module — comparable scope to what Beekeeb ships — see **[beekeeb/zmk-config-hshs](https://github.com/beekeeb/zmk-config-hshs)** and its West manifest; that layout is **not** interchangeable with `zmk module add` on this repo.

## Using as a User Config

Use **`config/`** as the West `self` path (see `config/west.yml`). Board and shield definitions for **HSHS46** are under `config/boards/shields/hshs46/`. Customize the layout by editing **`config/boards/shields/hshs46/hshs46.keymap`** (the build reads that path directly).

Push changes to trigger the GitHub Actions workflow (see `build.yaml`) and download firmware from the Actions tab.

## Hardware

- [HSHS46](https://beekeeb.com) — 46-key split keyboard. Uses a Pro Micro compatible controller (for example nice!nano v2) and supports the nice!view display.

Beekeeb also sells the **HSHS52**; this repo does not ship HSHS52 definitions or builds without hardware to validate them.
