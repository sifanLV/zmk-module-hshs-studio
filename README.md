# zmk-keyboards-hshs

ZMK keyboard module for the **HSHS46** and **HSHS52** split keyboards by [beekeeb](https://beekeeb.com).

This repository serves dual purpose:
- A **ZMK keyboard module** — add it to any ZMK config using `zmk module add`
- A **ZMK user config** — fork and use directly as your personal config

## Using as a Module (Recommended)

Run the following in your ZMK config repo:

```sh
zmk module add https://github.com/beekeeb/zmk-config-hshs
```

Then run `zmk keyboard add` and select **HSHS46** or **HSHS52** from the list.

Alternatively, add it manually to `config/west.yml`:

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: beekeeb
      url-base: https://github.com/beekeeb
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: zmk-config-hshs
      remote: beekeeb
      revision: main
  self:
    path: config
```

## Using as a User Config

Fork this repo, then edit the keymap files in `config/`:

- `config/hshs46.keymap` — keymap for HSHS46
- `config/hshs52.keymap` — keymap for HSHS52

Push your changes to trigger a GitHub Actions build. Download the firmware from the Actions tab.

## Hardware

- [HSHS46](https://beekeeb.com) — 46-key split keyboard
- [HSHS52](https://beekeeb.com) — 52-key split keyboard

Both use a Pro Micro compatible controller (e.g. nice!nano v2) and support the nice!view display.
