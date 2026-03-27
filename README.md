# Volume Knob (Zigbee2MQTT)

🇫🇷 [Version française](README.fr.md)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fvolume-knob-blueprint%2Fgh-pages%2Fen%2Fvolume_knob.yaml)

**Raw URL (copy-paste):**
```
https://raw.githubusercontent.com/nicolinuxfr/volume-knob-blueprint/gh-pages/en/volume_knob.yaml
```

This blueprint lets you control one or more media players using a Zigbee2MQTT rotary knob (e.g. IKEA SYMFONISK Sound Controller). Rotation adjusts volume with acceleration support, and button presses can be configured independently.

## How it works

1. The blueprint uses only the Home Assistant device selector.
2. Triggers are driven by the MQTT action events exposed by the selected device.
3. **Rotation** adjusts volume up or down. Rotation speed is read from the device's `action_step_size` and `action_rate` sensors, then translated into a repeat count (1 to 12 volume steps per event) for smooth acceleration.
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

- The selected device must expose MQTT action events together with `action_step_size` and `action_rate` sensors.
- Mute toggle checks each media player's mute state individually. If players have mixed mute states, each one will be toggled independently.
- The acceleration curve is tuned for the IKEA SYMFONISK. Other knobs may need different thresholds.
