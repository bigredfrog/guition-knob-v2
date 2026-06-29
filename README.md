# ESPHome Knob + HiFi Config Collection

This repository contains working ESPHome configurations for two hardware families:

- Guition 1.8-inch rotary touchscreen knob (ESP32-S3)
- Sonocotta HiFi ESP32-S3 audio boards

This is for general investigations, there is no matrix of support or implied maintanance.

[esp32-knob-simple.yaml](esp32-knob-simple.yaml), provides a hello world level touch + rotary UI for Sendspin playback, album art display, haptics, and LED ring feedback.

Files relating to esp32-hifi were generally used to investigate networking disconnects.

## Hardware Platform Links

- Guition Knob Display: https://www.guition.com/esp32-display-module/1-8-inch-knob-display
- Sonocotta HiFi ESP32/ESP32-S3: https://sonocotta.com/hifi-esp32-and-hifi-esp32s3/

## Original Code sources

https://github.com/RealDeco/sendspin-guition
https://github.com/sonocotta/esp32-audio-dock/tree/main/firmware/esphome/1-hifi-esp32

## Top-Level Capability Overview

Primary capabilities in [esp32-knob-simple.yaml](esp32-knob-simple.yaml):

- Sendspin playback integration (title, artist, group transport, album art decode)
- Large touchscreen UI built with LVGL (controls page, now playing page, demo pages)
- Rotary encoder input with context-aware behavior
- Haptic click feedback via DRV2605
- Dynamic album art handling with fallback and transition animation
- WS2812 LED ring state indication with simple animation
- I2S speaker output plus microphone support

## UI Flow (esp32-knob-simple.yaml)

Boot and navigation flow:

1. Boot page is shown briefly.
2. UI enters Controls mode by default.
3. Tap in Controls mode switches to Now Playing.
4. Tap in Now Playing switches back to Controls.
5. Rotate knob in Controls mode adjusts volume with overlay feedback.
6. Rotate knob in Now Playing or Demo modes browses pages in a circular sequence.
7. Swipe in Now Playing:
	- Left/Right: next/previous track
	- Up/Down: volume up/down
8. Tap in Demo pages shows local touch indicator feedback.

UI phases used by this config:

- Controls
- Now Playing
- Demo 1
- Demo 2
- Demo 3
- Demo 4

## YAML Script Guide

- [esp32-knob-simple.yaml](esp32-knob-simple.yaml): Primary Guition knob build with focused media UI flow, touch gestures, rotary control, haptics, and LED feedback.
- [esp32-knob-ddp.yaml](esp32-knob-ddp.yaml): Adds local DDP and DDP canvas rendering support to the simplified knob baseline.
- [esp32-knob.yaml](esp32-knob.yaml): Full-featured knob configuration with additional standby/playlist behavior and DDP integration.
- [esp32-hifi-wifi.yaml](esp32-hifi-wifi.yaml): Sonocotta HiFi board profile using Wi-Fi and package-based Sendspin/audio modules.
- [esp32-hifi-wifi-black.yaml](esp32-hifi-wifi-black.yaml): Alternate Wi-Fi HiFi device profile (board/color variant) with the same package-driven stack.
- [esp32-hifi-eth.yaml](esp32-hifi-eth.yaml): HiFi profile oriented to Ethernet package usage instead of Wi-Fi.

## Build And Run

Build, Run and Log the primary knob config with:

```bash
esphome run esp32-knob-simple.yaml
```

## secrets.yaml

This repository uses [secrets.yaml](secrets.yaml) for local credentials and sensitive values referenced by `!secret` keys in the YAML configs.

Copy, rename and populate secrets.yaml from the template of [secrets_example.yaml](secrets_example.yaml)

Keep [secrets.yaml](secrets.yaml) local and do not commit private credentials.
