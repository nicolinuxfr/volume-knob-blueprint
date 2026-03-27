# Volume Knob (Zigbee2MQTT)

🇫🇷 [Version française](README.fr.md)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fvolume-knob-blueprint%2Fgh-pages%2Fen%2Fvolume_knob.yaml)

**Raw URL (copy-paste):**
```
https://raw.githubusercontent.com/nicolinuxfr/volume-knob-blueprint/gh-pages/en/volume_knob.yaml
```

This blueprint lets you control one or more media players using a Zigbee2MQTT rotary knob (e.g. IKEA SYMFONISK Sound Controller). Rotation adjusts volume with acceleration support, and button presses can be configured independently.

## How it works

1. The blueprint mirrors a working Zigbee2MQTT MQTT automation: it listens on the base topic for rotation events and on the `/action` subtopic for button presses.
2. The Home Assistant device selector is still present. By default, the blueprint derives the base topic from the selected device name as `zigbee2mqtt/<device name>`.
3. If your Home Assistant device name does not exactly match the Zigbee2MQTT topic, you can set an explicit MQTT topic override.
4. **Rotation** adjusts volume up or down. The speed of rotation is detected via `action_step_size` and `action_rate` from the MQTT payload, and translated into a repeat count (1 to 12 volume steps per event) for smooth acceleration.
5. **Button presses** (single click, double click, long press) are each mapped to a configurable action: toggle mute, toggle power, play/pause, or no action.
6. Volume and mute/play actions only target media players that are currently available (not off, unknown, or unavailable). The toggle action targets all configured players, so you can turn on an off player.

## Configuration options

| Input | Description | Default |
|---|---|---|
| Zigbee2MQTT device | The Z2M rotary knob to use (selected from your MQTT devices) | — |
| MQTT topic override | Optional exact base topic if auto-detection from the device name is wrong | Empty |
| Media player(s) | One or more media players to control | — |
| Single click | Action on single click | Toggle mute |
| Double click | Action on double click | Play / Pause |
| Long press | Action on long press | Toggle on/off |

## Supported devices

This blueprint works with any Zigbee2MQTT rotary encoder that sends `brightness_step_up`/`brightness_step_down` rotation events and `toggle`/`double`/`hue_move` button events. Known compatible devices:

- IKEA SYMFONISK Sound Controller (E1744)
- Other Z2M rotary encoders with the same payload format

## Known limitations

- The default topic auto-detection assumes the Home Assistant device name exactly matches the Zigbee2MQTT topic suffix.
- If nothing happens, set the MQTT topic override explicitly, for example `zigbee2mqtt/Salon - molette ampli`.
- Mute toggle checks each media player's mute state individually. If players have mixed mute states, each one will be toggled independently.
- The acceleration curve is tuned for the IKEA SYMFONISK. Other knobs may need different thresholds.
