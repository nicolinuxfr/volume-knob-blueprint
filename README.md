# Volume Knob (Zigbee2MQTT)

🇫🇷 [Version française](README.fr.md)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fvolume-knob-blueprint%2Fgh-pages%2Fen%2Fvolume_knob.yaml)

**Raw URL (copy-paste):**
```
https://raw.githubusercontent.com/nicolinuxfr/volume-knob-blueprint/gh-pages/en/volume_knob.yaml
```

This blueprint lets you control one or more media players using a Zigbee2MQTT rotary knob (e.g. IKEA SYMFONISK Sound Controller). Rotation adjusts volume with acceleration support, and button presses can be configured independently.

## How it works

1. The blueprint keeps the Home Assistant device selector and primarily listens through Zigbee2MQTT MQTT device triggers tied to that selected device.
2. It also listens to the raw MQTT topics `zigbee2mqtt/<friendly_name>` and `zigbee2mqtt/<friendly_name>/action` as a fallback path, which preserves detailed rotation payload data when available.
3. **Rotation** adjusts volume up or down. When the raw MQTT payload is available, the speed of rotation is detected via `action_step_size` and `action_rate`, then translated into a repeat count (1 to 12 volume steps per event) for smooth acceleration. When only the device trigger fires, rotation still works with the base step.
4. **Button presses** (single click, double click, long press) are each mapped to a configurable action: toggle mute, toggle power, play/pause, or no action.
5. Volume and mute/play actions only target media players that are currently available (not off, unknown, or unavailable). The toggle action targets all configured players, so you can turn on an off player.

## Configuration options

| Input | Description | Default |
|---|---|---|
| Zigbee2MQTT device | The Z2M rotary knob to use (selected from your MQTT devices) | — |
| Media player(s) | One or more media players to control | — |
| Single click | Action on single click | Toggle mute |
| Double click | Action on double click | Play / Pause |
| Long press | Action on long press | Toggle on/off |

## Supported devices

This blueprint works with any Zigbee2MQTT rotary encoder that sends `brightness_step_up`/`brightness_step_down` rotation events and `toggle`/`double`/`hue_move` button events. Known compatible devices:

- IKEA SYMFONISK Sound Controller (E1744)
- Other Z2M rotary encoders with the same payload format

## Known limitations

- The Z2M device is selected via the HA device picker (filtered to MQTT devices). MQTT device triggers are the primary trigger path. The raw MQTT topic fallback still assumes the Home Assistant device name matches the Zigbee2MQTT friendly name.
- Zigbee2MQTT discovers MQTT device triggers only after the corresponding action has been emitted at least once on the device.
- Mute toggle checks each media player's mute state individually. If players have mixed mute states, each one will be toggled independently.
- The acceleration curve is tuned for the IKEA SYMFONISK. Other knobs may need different thresholds.
