# Getting Started With coroNET OS 1

> **Please do not pay for a new coroNET OS 1 activation.** We recommend
> [coroNET OS 2](https://github.com/AlphaStudioDE/coroNET_OS_2), a complete
> ground-up firmware rewrite available to everyone free of charge under the
> [MIT License](https://github.com/AlphaStudioDE/coroNET_OS_2/blob/main/LICENSE),
> with no activation fees. Follow the OS 2 installation instructions when upgrading.
>
> Thank you to everyone who supported coroNET OS 1 through an activation payment
> in the past. Your support helped make the next chapter possible.

This guide is the short path for building, flashing, activating, and starting a coroNET OS 1 device for the first time.

For detailed wiring, use [ASSEMBLY.md](ASSEMBLY.md). For the full bill of materials, use [BOM.md](BOM.md). For flashing details, use [FLASHING.md](FLASHING.md).

---

## 1. Print The Parts

Print the standard mechanical parts from:

[coroNET.3mf](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf)

Optional community mod:

[Alternative Magnetic Enclosure](mods/alternative-magnetic-enclosure) by **@wlodeka** on Discord.

The `.3mf` file contains the printable parts and print settings for the standard enclosure.

---

## 2. Prepare The Hardware

Use [BOM.md](BOM.md) as the hardware checklist.

Core parts:

- JC3248W535 ESP32-S3 touchscreen controller
- SK6812 RGBNW LED strip, 1 m / 60 LEDs / IP20
- 5 V / 10 A power supply
- 4 ohm / 3 W speaker
- 5 V PWM fan, recommended Noctua NF-A4x10 5 V PWM or suitable replacement
- servo for the ventilation flap
- 1000 uF / 10 V capacitor near LED power input
- optional 10 A fuse / inline protection component
- optional SN74AHCT125N LED data buffer
- wiring, connectors, screws, printed enclosure parts

Some common workshop parts, such as wires, screws, connectors, or small electronics, may already be available and can reduce the actual build cost.

---

## 3. Assemble And Wire

Follow [ASSEMBLY.md](ASSEMBLY.md).

Main wiring summary:

| Function | Connection |
| --- | --- |
| LED data | ESP32-S3 `GPIO15` to LED `DIN` |
| Fan PWM/control | ESP32-S3 `GPIO14` |
| Servo signal | ESP32-S3 `GPIO9` |
| Speaker | Board speaker output, not directly to GPIO |
| LED power | 5 V power distribution to LED `5V/GND` |
| LED capacitor | 1000 uF / 10 V across LED `5V/GND`, close to LED input |
| Common ground | All powered modules must share common ground |

Recommended wire colors in the assembly guide:

- red: `5V`
- white: `GND`
- green: LED data
- yellow: fan PWM/control
- brown: flap servo signal
- blue: audio path in the diagram; the physical speaker wires may use their own colors

---

## 4. Flash The Base Firmware

Download the base wired-flash package:

[Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)

Download Espressif Flash Download Tool from the official Espressif website:

[Download Espressif Flash Download Tools](https://www.espressif.com/en/tools-type/flash-download-tools)

Then follow [FLASHING.md](FLASHING.md).

Current public base package:

- `coronet_bootloader.bin`
- `coronet_partitions.bin`
- `boot_app0.bin`
- `firmware.bin`

Flash Download Tool settings:

- ChipType: `ESP32-S3`
- WorkMode: `Develop`
- LoadMode: `UART`
- SPI SPEED: `80MHz`
- SPI MODE: `DIO`
- Flash Size: `16MB` / `128Mbit`
- Baud: start with `460800`; use `115200` if flashing fails

---

## 5. Prepare The SD Card

Use the included 500 MB microSD card where possible. If it is missing, use a 500 MB to 2 GB card. Avoid larger cards unless tested with the target ESP32-S3 board.

Optional root-level files:

| File | Purpose |
| --- | --- |
| `boot.wav` | Startup audio for the boot animation |
| `license.txt` | Activation file, if already received |
| `firmware.bin` | SD firmware update file, only when intentionally using SD update |

---

## 6. First Boot

After flashing:

1. Insert the prepared SD card.
2. Power-cycle the device.
3. Confirm that the boot screen appears.
4. Confirm that the boot LED animation starts.
5. If `boot.wav` is present, confirm that boot audio plays.
6. If the activation screen appears, continue with activation.
7. Configure WiFi and printer connection after activation.
8. Check LED layout and enable Mirror LED mode only if the physical LED direction requires it.
9. Test fan and servo operation before daily use.

---

## 7. Activate The Device

coroNET OS 1 requires a device-specific activation key.

New activation payments are discouraged; use the free MIT-licensed [coroNET OS 2](https://github.com/AlphaStudioDE/coroNET_OS_2) instead. The instructions below are for existing OS 1 activation-key holders. OS 1's activation mechanism and license have not changed.

Activate using one of these methods:

- copy the received activation file, usually `license.txt`, to the SD card root folder, insert the SD card, and restart the device
- manually enter the received activation code on the activation screen

The activation key is generated for the displayed serial number. Do not use a serial number from another device.

---

## 8. Configure Printer And Features

After activation:

- configure WiFi
- configure printer / Moonraker connection
- check home screen telemetry
- set LED brightness, inside WHITE/AMBIENT mode, Mirror LED if needed, and DIMM behavior
- set sound volume and sound scenarios
- check ventilation mode, fan PWM range, and servo open/closed positions
- verify OTA update status in the system page

---

## 9. Future Updates

The base wired flash is intended for first-time installation or recovery.

After coroNET OS 1 `1.6.0` is installed, future updates should normally be installed through the built-in OTA updater.

Firmware releases are available here:

[Latest release](https://github.com/AlphaStudioDE/coroNET_OS_1/releases/latest)

---

## Quick Troubleshooting

| Problem | Try |
| --- | --- |
| Flash tool cannot connect | Hold `BOOT`, press and release `RESET`, release `BOOT`, then click `START` |
| Flashing fails | Lower baud rate to `115200`, change USB cable, close serial monitors |
| Boot audio does not play | Check `boot.wav` in SD root, SD card size, and speaker connection |
| Activation does not apply from SD | Confirm `license.txt` is in SD root and restart the device |
| LEDs show wrong direction | Enable or disable Mirror LED mode in the LED tab |
| Fan or servo behaves incorrectly | Recheck `GPIO14` fan PWM, `GPIO9` servo signal, power, and ground |
| Device resets under load | Check 5 V power supply, common ground, LED capacitor, fan/servo current, and wiring |
