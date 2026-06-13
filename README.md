# coroNET OS 1

[![Firmware](https://img.shields.io/badge/firmware-1.6.0-blue)](https://github.com/AlphaStudioDE/coroNET_OS_1/releases/latest)
![Status](https://img.shields.io/badge/status-stable%20%2F%20out%20of%20beta-brightgreen)
![Platform](https://img.shields.io/badge/platform-ESP32-orange)
![License](https://img.shields.io/badge/license-proprietary-lightgrey)

**coroNET OS 1** is a dedicated firmware and interface system for a custom 3D-printer companion device built around the Snapmaker U1 workflow. It brings printer status, LED animation, sound feedback, ventilation control, OTA updates, diagnostics, and a polished touchscreen UI into one integrated controller.

The project has now moved out of beta. Development continues, but the system is considered stable enough to represent the first public product direction.

> Current firmware: **1.6.0**

**Quick links:** [Latest release](https://github.com/AlphaStudioDE/coroNET_OS_1/releases/latest) | [First-time flash files](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip) | [Printable parts](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf) | [Assembly guide](ASSEMBLY.md) | [Flashing guide](FLASHING.md) | [Activation](#activation) | [Project status](#project-status) | [Features](#main-features) | [Hardware notes](#hardware-notes) | [Safety notes](#safety-notes)

---

## First-Time Installation

For a new device or a full recovery flash, download:

[Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)

This package contains the base wired-flash files for coroNET OS 1 `1.6.0`. After the base firmware is installed, future updates are handled through the built-in OTA updater.

You also need Espressif Flash Download Tool:

[Download official Espressif Flash Download Tools](https://www.espressif.com/en/tools-type/flash-download-tools)

Then follow the [flashing guide](FLASHING.md).

---

## Activation

coroNET OS 1 firmware requires a device-specific activation key.

On first startup, the device shows its serial number on the activation screen. To receive an activation key, support the project with a **20 EUR** PayPal contribution:

```text
PayPal: damianborkowski88@gmail.com
Payment note: your displayed device serial number
```

The activation file or activation code will be sent to the email address connected to the PayPal payment.

Activation can be completed in either of these ways:

- copy the received activation file, usually `license.txt`, to the root folder of the SD card, insert the SD card into the ESP32-S3 device, and restart the device
- manually enter the received activation code on the activation screen

The activation key is generated for the displayed device serial number. Do not use a serial number from a different device.

---

## Printable Parts

The printable mechanical parts and print settings are provided as:

[coroNET.3mf](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf)

Use this file as the base print project for the enclosure and related printed parts.

Optional community mod:

[Alternative Magnetic Enclosure](mods/alternative-magnetic-enclosure) by **@wlodeka** on Discord.

---

## What It Does

coroNET OS 1 turns printer telemetry into a physical, ambient interface:

- shows printer status, progress, tool, material, time, ETA, temperatures, and connectivity
- controls a 60 LED RGBW lighting system split into outer and inside sections
- reacts to print states with status-specific LED animations
- plays sound feedback for boot, printer events, errors, and user actions
- provides OTA firmware updates through GitHub Releases
- controls ventilation, servo flap, fan PWM, chamber temperature behavior, and safety fallback modes
- includes touchscreen UI pages for home, LED, sound, system, OTA, ventilation, clock, and setup
- supports screen saver clocks, display dimming, silent modes, diagnostics, and activation handling

---

## Project Status

**Stable release line:** `1.6.x`

The firmware is no longer treated as beta. The current focus is:

- polishing product presentation
- preparing documentation
- preparing launch material
- validating real hardware builds beyond the wired prototype stage
- improving animation quality, UX consistency, and long-term stability

---

## Main Features

### Printer-Aware LED System

- 60 RGBW LEDs
- 42 outer LEDs and 18 inside LEDs
- section-based layout: left, center/front, right, inside
- WHITE / AMBIENT inside color modes
- Ambilight-style inside sampling from nearby outer LEDs
- per-section brightness control
- per-section inactivity DIMM
- Mirror LED layout option for units wired in the previous direction
- animation preview in the LED tab
- Color Remix for non-semantic animations
- status-specific animation banks for idle, print, pause, error, finish, and other modes

### Intelligent PRINT Animations

PRINT animations can use real printer data such as:

- active filament color
- active tool temperature
- bed temperature
- chamber temperature
- print progress
- printer status
- connectivity and diagnostics

Examples include:

- Progress Bar
- Laser
- Wave
- Thermal
- Stripes
- Thermometer
- Snake
- Rainbow Progress
- Layer Fill
- Pixel Rain
- Thermal Balance
- Material Core
- Heat Soak
- Stability Monitor
- Time Flow
- Quality Guard
- Nozzle Heat

### Boot Experience

- 35-second startup LED show
- designed as a cinematic startup sequence, not a simple looping animation
- supports full RGBW inside and outer effects during boot
- can sync with `/boot.wav` from the SD card
- boot audio may continue naturally after the LED boot animation hands off to normal status mode
- boot brightness follows saved LED brightness preferences
- boot volume follows saved sound volume preferences

### Sound System

- SD-card based WAV playback
- boot audio support through `/boot.wav`
- per-scenario sound configuration
- sound enable/disable
- volume control
- repeat options
- event-driven feedback for printer states
- safeguards to avoid blocking critical UI/LED operation during boot

### OTA Updates

- GitHub Releases based firmware update flow
- in-device update check
- reinstall support for the same version when needed
- OTA resource preparation before flashing
- printer communication pause/recovery around update operations
- current and available version reporting

### Ventilation And Chamber Control

- servo flap control
- fan PWM control
- chamber temperature monitoring
- automatic and manual modes
- failsafe behavior
- chamber heater support options
- Panda / external ventilation-related controls where configured

### Touchscreen UI

- home dashboard
- LED configuration and preview
- sound browser and scenario configuration
- system / OTA tools
- setup wizard
- ventilation page
- clock and screen saver modes
- diagnostics and status indicators
- multiple UI themes and clock styles

---

## Hardware Notes

The firmware is intended for the coroNET OS 1 controller hardware and related prototype/product builds.

### Hardware / BOM Overview

A typical coroNET OS 1 build includes:

- ESP32-S3 touchscreen controller
- 60 RGBW SK6812 LEDs
- SD card for sound assets and activation files
- onboard audio output and speaker
- PWM-controlled fan
- servo-driven ventilation flap
- chamber temperature data from printer telemetry
- custom enclosure, wiring, connectors, and power distribution

Printable enclosure parts and print settings are available in [coroNET.3mf](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf).

An optional magnetic enclosure mod is available here: [Alternative Magnetic Enclosure](mods/alternative-magnetic-enclosure), shared by **@wlodeka** on Discord.

For a detailed and revision-specific bill of materials, see [BOM.md](BOM.md).

For wiring, GPIO mapping, and assembly order, see [ASSEMBLY.md](ASSEMBLY.md).

Current LED layout:

- `0..10` right outer section
- `11..30` center/front outer section
- `31..41` left outer section
- `42..59` inside section

Mirror LED mode can be enabled in the LED tab for units wired in the previous direction.

Important GPIO usage is defined in the firmware and may vary between hardware revisions. Always check the firmware configuration before wiring a new unit.

---

## SD Card Files

The SD card can be used for sound assets and activation files.

Common root-level files:

- `boot.wav` - optional startup audio played during the boot animation
- `license.txt` - activation/license file copied to the SD root folder when automatic activation is used

Sound scenario files are selected through the device UI.

---

## Releases

Firmware binaries are published through GitHub Releases:

[Latest release](https://github.com/AlphaStudioDE/coroNET_OS_1/releases/latest)

The device OTA updater reads the release manifest and firmware binary from the release assets.

For first-time installation or recovery flashing with Espressif Flash Download Tool, see [FLASHING.md](FLASHING.md).

Base wired-flash package:

[Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)

---

## Repository Purpose

This repository is primarily used as the public firmware update and release channel for coroNET OS 1.

It is not intended to be a general open-source firmware project or a drop-in firmware for unrelated hardware.

---

## Compatibility

Developed for:

- coroNET OS 1 hardware
- Snapmaker U1-oriented workflow
- Moonraker-style printer telemetry
- ESP32-based touchscreen controller builds used by the project

Compatibility may depend on hardware revision, wiring, SD card setup, printer firmware, network configuration, and release version.

---

## Safety Notes

coroNET OS 1 can control LEDs, audio, fans, servo movement, ventilation behavior, and update flow. Before daily use:

- verify fan and servo directions
- verify failsafe values
- verify LED section mapping
- test error/status behavior
- test OTA only with stable power
- keep a known-good firmware binary available

---

## Credits

Special thanks to **@wlodeka** on Discord for testing, feedback, and sharing the optional [Alternative Magnetic Enclosure](mods/alternative-magnetic-enclosure) community mod.

---

## Legal / License

Copyright (c) 2026 Damian Borkowski / AlphaStudioDE.

All rights reserved.

The firmware, design, project files, and related assets are proprietary unless explicitly stated otherwise. This repository is provided for firmware distribution, update delivery, product documentation, and project presentation.

Snapmaker and other product names may be trademarks of their respective owners. coroNET OS 1 is an independent project and is not affiliated with or endorsed by Snapmaker.

---

## Author

Created by **Damian Borkowski / AlphaStudioDE**.

Project status: **out of beta, stable product direction**
