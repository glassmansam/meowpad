# Meowpad

A small collection of board and shield configuration files for the Meowpad project (ZMK / Zephyr based keyboard configs).

## Overview

This repository contains board/shield definitions, overlay and keymap files, and a minimal Zephyr module layout used to build firmware for Meowpad-compatible hardware.

## Quick Start

Prerequisites:
- Install Zephyr SDK and `west` per Zephyr Project instructions.
- Install ZMK build dependencies if you plan to use ZMK.

Basic local setup:

```bash
# from repo root
west update
```

Basic local build for Meowpad:

```bash
# standard firmware
west build -d build/meowpad -s zephyr -b nice_nano_v2 -- -DSHIELD=meowpad

# ZMK Studio firmware
west build -d build/meowpad_studio -s zephyr -b nice_nano_v2 \
  -S studio-rpc-usb-uart -- -DSHIELD=meowpad -DCONFIG_ZMK_STUDIO=y
```

Consult the ZMK documentation for flashing steps for your controller.

## Files of interest

- [config/west.yml](config/west.yml)
- [boards/shields/meowpad/meowpad.zmk.yml](boards/shields/meowpad/meowpad.zmk.yml)
- [boards/shields/meowpad/meowpad.keymap](boards/shields/meowpad/meowpad.keymap)
- [boards/shields/meowpad/meowpad.overlay](boards/shields/meowpad/meowpad.overlay)
- [boards/shields/meowpad/meowpad.conf](boards/shields/meowpad/meowpad.conf)
- [zephyr/module.yml](zephyr/module.yml)

## Current firmware setup

- `build.yaml` produces both a normal `meowpad` build and a `meowpad_with_studio` build with `CONFIG_ZMK_STUDIO=y`.
- The keymap includes:
  - a `Base` layer,
  - an `Fn` layer for editing shortcuts, Bluetooth access, and ZMK Studio unlock,
  - a `Bluetooth` layer with direct access to ZMK profiles 0-4 plus USB/BLE output switching,
  - `YouTube` and `VLC/mpv` media layers,
  - an `Edit` layer for common desktop editing shortcuts,
  - `Onshape`, `OS Sketch`, and `OS View` layers,
  - three RGB encoder layers for brightness, hue, and effect control,
  - two reserved Studio-only expansion layers.

## ZMK Studio

### What to flash

Use the Studio-enabled firmware if you want runtime remapping in ZMK Studio:

- GitHub Actions artifact: `meowpad_with_studio`
- Local build: use the `studio-rpc-usb-uart` snippet and `-DCONFIG_ZMK_STUDIO=y`

The plain `meowpad` build does not expose the Studio connection endpoint.

### How to unlock Studio

ZMK Studio changes are gated behind the `&studio_unlock` behavior.

On the current Meowpad keymap, unlock it like this:

1. Hold the `fn` key on the base layer.
2. While holding it, press the bottom-right key (`m4` position).

That key is bound to `&studio_unlock` on the `Fn` layer. After pressing it, the board stays unlocked until ZMK Studio disconnects or times out from inactivity.

### How to connect

You can connect with:

- the web app: `https://zmk.studio/` in Chrome or Edge
- the native app for Linux, macOS, or Windows from the ZMK Studio download page

USB:

1. Flash the `meowpad_with_studio` firmware.
2. Plug the board in over USB.
3. If needed, switch keyboard output to USB before connecting from Studio.
4. Unlock Studio from the keyboard.
5. Connect from ZMK Studio and grant serial access if your OS asks.

BLE:

1. Flash the `meowpad_with_studio` firmware.
2. Pair the board to your computer over Bluetooth.
3. Switch keyboard output to BLE before connecting from Studio.
4. Unlock Studio from the keyboard.
5. Connect from ZMK Studio.

Note: ZMK docs say USB and BLE endpoints should match the Studio connection you use. For example, if you connect to Studio over USB, the keyboard output should also be USB.

### Important Studio caveat

Once you start editing the keymap in ZMK Studio, later changes to `boards/shields/meowpad/meowpad.keymap` will not take effect on the device unless you use `Restore Stock Settings` in ZMK Studio first.

In practice:

- If you want to keep editing the keymap in Git, do that before switching to Studio-based editing.
- If you want to keep using Studio, leave the `.keymap` file for structural changes only, such as adding new empty layers or new custom behaviors.

## Current key usage

### Base layer

Physical legend:

```text
| knob | fn | 0 | m1 |
|  1   | 2  | 3 | m2 |
|  4   | 5  | 6 | m3 |
|  7   | 8  | 9 | m4 |
```

Current behavior:

- Encoder rotate: volume up/down
- `knob` press: mute/unmute
- `fn`: momentary `Fn` layer
- `0` to `9`: numeric keypad-style entry
- `m1`: jump to `YouTube`
- `m2`: jump to `Onshape`
- `m3`: jump to `Edit`
- `m4`: `Enter`

### Fn layer

The `Fn` layer provides:

- `Esc`, `Backspace`, and `Delete`
- undo, redo, copy, paste, cut, select all, find, and save
- screenshot and terminal-launch shortcuts
- a jump to the `Bluetooth` layer
- `&studio_unlock` on the bottom-right key

### Bluetooth layer

The `Bluetooth` layer provides:

- direct profile select for Bluetooth profiles `0` through `4`
- next/previous profile selection
- clear current profile and clear all profiles
- USB/BLE output selection

### Media layers

The right-side mode keys on the base layer open two media-focused layers:

- `YouTube`: play/pause, mute, seek keys, fullscreen, and direct volume keys
- `VLC/mpv`: play/pause, mute, seek, fullscreen, bracket controls, and fine navigation keys

Both media layers keep encoder rotation on volume.

### Edit layer

The `Edit` layer mirrors the editing shortcuts from `Fn`, but gives them a dedicated workflow page with `Backspace`, `Delete`, `Space`, and `Enter` on the bottom row.

### Onshape layers

The Onshape stack is split into three layers:

- `Onshape`: main modeling shortcuts and a hold key for the view layer
- `OS Sketch`: sketch tools
- `OS View`: view and camera shortcuts

These layers swap the encoder from volume to zoom behavior.

### RGB encoder layers

Three RGB layers are present for Studio or manual activation:

- `RGB Bright`: encoder controls brightness
- `RGB Hue`: encoder controls hue
- `RGB Effect`: encoder controls effect selection

All three share the same button layout for RGB toggle and RGB parameter keys; only the encoder rotation behavior changes.

## Contributing

Feel free to open issues or PRs to update keymaps, overlays, or add new board support.

## License

Project files inherit the license of their upstream projects; include an explicit license file if you want a single repo-wide license.
