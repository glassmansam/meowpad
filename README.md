# Meowpad

A small collection of board and shield configuration files for the Meowpad project (ZMK / Zephyr based keyboard configs).

## Overview

This repository contains board/shield definitions, overlay and keymap files, and a minimal Zephyr module layout used to build firmware for Meowpad-compatible hardware.

## Quick Start

Prerequisites:
- Install Zephyr SDK and `west` per Zephyr Project instructions.
- Install ZMK build dependencies if you plan to use ZMK.

Basic build (example):

```bash
# from repo root
west update
west build -s zephyr -b nice_nano_nrf52840_zmk
```

Adjust the board name to match your target. Consult the ZMK documentation for flashing steps.

## Files of interest

- [config/west.yml](config/west.yml)
- [boards/shields/meowpad/meowpad.zmk.yml](boards/shields/meowpad/meowpad.zmk.yml)
- [boards/shields/meowpad/meowpad.keymap](boards/shields/meowpad/meowpad.keymap)
- [boards/shields/meowpad/meowpad.overlay](boards/shields/meowpad/meowpad.overlay)
- [boards/shields/meowpad/meowpad.conf](boards/shields/meowpad/meowpad.conf)
- [zephyr/module.yml](zephyr/module.yml)

## Contributing

Feel free to open issues or PRs to update keymaps, overlays, or add new board support.

## License

Project files inherit the license of their upstream projects; include an explicit license file if you want a single repo-wide license.
