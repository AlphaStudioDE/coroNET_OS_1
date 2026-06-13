# Flashing coroNET OS 1 With Espressif Flash Download Tool

This guide explains how to flash coroNET OS 1 firmware to the ESP32-S3 controller using Espressif Flash Download Tool on Windows.

Official Espressif references:

- [Flash Download Tools](https://www.espressif.com/en/tools-type/flash-download-tools)
- [ESP32-S3 Flash Download Tool User Guide](https://docs.espressif.com/projects/esp-test-tools/en/latest/esp32s3/production_stage/tools/flash_download_tool.html)

---

## Required Files

Download the current public first-time flashing package:

[Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)

This ZIP contains the base wired-flash files for coroNET OS 1 `1.6.0`:

- `coronet_bootloader.bin`
- `coronet_partitions.bin`
- `boot_app0.bin`
- `firmware.bin`
- `readME.txt`

For normal OTA releases, coroNET OS 1 publishes:

- `firmware.bin` - application firmware used by the built-in OTA updater

For first-time flashing with Flash Download Tool, use one of the following:

### Alternative: Single Factory Image

- `coroNET_OS_1_<version>_merged.bin`

This is an alternative packaging option. It contains the bootloader, partition table, boot app image, and application firmware in one file.

Build output source:

- `sketch_v20_spi.ino.merged.bin`

Flash address:

- `0x0`

### Current Public Package: Split Images

Use this with the public [Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip) package.

| Address | File |
| ---: | --- |
| `0x0` | `coronet_bootloader.bin` |
| `0x8000` | `coronet_partitions.bin` |
| `0xe000` | `boot_app0.bin` |
| `0x10000` | `firmware.bin` or `sketch_v20_spi.ino.bin` |

Current build flash settings:

- Flash mode: `DIO`
- Flash frequency: `80 MHz`
- Flash size: `16 MB` / `128 Mbit`

---

## Before Flashing

1. Use a good USB data cable, not a charge-only cable.
2. Connect the ESP32-S3 controller to the PC.
3. Close Arduino IDE Serial Monitor, other serial terminals, and any software that may use the COM port.
4. Download and unzip the official Espressif Flash Download Tool:
   - [Download Espressif Flash Download Tools](https://www.espressif.com/en/tools-type/flash-download-tools)
5. Download and unzip the coroNET OS 1 first-time flashing files:
   - [Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)
6. Prepare the SD card after flashing:
   - place `boot.wav` in the SD card root if boot audio is used
   - place `license.txt` in the SD card root if you already received an activation file

Recommended SD card size: use the included 500 MB card where possible. If it is missing, use a 500 MB to 2 GB card. Avoid larger cards unless tested with the target ESP32-S3 board.

---

## Activation After Flashing

coroNET OS 1 requires a device-specific activation key.

On first startup, the device shows its serial number on the activation screen. To receive an activation key, support the project with a **20 EUR** PayPal contribution:

```text
PayPal: damianborkowski88@gmail.com
Payment note: your displayed device serial number
```

The activation file or activation code will be sent to the email address connected to the PayPal payment.

You can activate the device in either of these ways:

- copy the received activation file, usually `license.txt`, to the root folder of the SD card, insert the SD card into the ESP32-S3 device, and restart the device
- manually enter the received activation code on the activation screen

The activation key is generated for the displayed device serial number. Do not use a serial number from a different device.

---

## Flash Download Tool Settings

1. Start Flash Download Tool.
2. Select:
   - ChipType: `ESP32-S3`
   - WorkMode: `Develop`
   - LoadMode: `UART`
3. Click `OK`.
4. In the main window, select:
   - SPI SPEED: `80MHz`
   - SPI MODE: `DIO`
   - Flash Size: `16MB` / `128Mbit`
   - COM: the COM port of the connected ESP32-S3
   - BAUD: start with `460800`; if flashing fails, use `115200`

Do not enable encrypted download, secure boot, or flash encryption for normal coroNET OS 1 flashing.

---

## Option A: Flash One Merged File

Use this only if your package includes a merged factory image.

1. In the first file row, select:
   - file: `coroNET_OS_1_<version>_merged.bin`
   - address: `0x0`
2. Tick the checkbox for that row.
3. Click `ERASE` if this is a fresh install or recovery flash.
4. Wait for erase to finish.
5. Click `START`.
6. Wait until the tool reports a successful download.
7. Press `RESET` or power-cycle the device.

This method may erase existing WiFi, UI, LED, sound, and device settings.

---

## Option B: Flash Current Public ZIP

Use this for the current public [Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip) package.

Add these rows and tick each one:

| Address | File |
| ---: | --- |
| `0x0` | `coronet_bootloader.bin` |
| `0x8000` | `coronet_partitions.bin` |
| `0xe000` | `boot_app0.bin` |
| `0x10000` | `firmware.bin` |

Then:

1. Click `ERASE` for a fresh install or recovery.
2. Click `START`.
3. Wait until flashing is complete.
4. Press `RESET` or power-cycle the device.

---

## Option C: Application-Only Flash

Use this only when the controller already has the correct coroNET OS 1 bootloader and partition table.

| Address | File |
| ---: | --- |
| `0x10000` | `firmware.bin` |

Do not use this for a blank ESP32-S3.

Application-only flashing usually keeps existing NVS settings, but if the firmware layout changed, a full merged flash is safer.

---

## If The Device Is Not Detected

Try this bootloader entry sequence:

1. Hold `BOOT`.
2. Press and release `RESET`.
3. Release `BOOT`.
4. Click `START` again in Flash Download Tool.

If it still fails:

- try another USB cable
- try another USB port
- reduce baud rate to `115200`
- close all programs using the COM port
- reconnect power and USB
- check Windows Device Manager for the correct COM port

---

## First Boot Checklist

After flashing:

1. Insert the prepared SD card.
2. Power-cycle the device.
3. Confirm that the boot screen appears.
4. Confirm that the boot LED animation starts.
5. If `boot.wav` is present, confirm that boot audio plays.
6. If the activation screen is shown, complete activation with `license.txt` on the SD card or by manually entering the activation code.
7. Configure WiFi and printer connection if settings were erased.
8. Check LED layout and enable Mirror LED mode only if the physical LED direction requires it.

---

## Public File Distribution

The repository root contains the first-time wired flashing package:

- [Files to flash.zip](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/Files%20to%20flash.zip)

This package is intended as the base installer for coroNET OS 1 `1.6.0`. After it is installed, later firmware updates should normally be installed through the built-in OTA updater.

The ZIP should contain:

- `coronet_bootloader.bin`
- `coronet_partitions.bin`
- `boot_app0.bin`
- `firmware.bin`
- `readME.txt`

Do not redistribute Espressif Flash Download Tool inside this repository. Link users to the official Espressif download page instead:

- [Download Espressif Flash Download Tools](https://www.espressif.com/en/tools-type/flash-download-tools)

OTA release assets should still include:

- `firmware.bin` - used by OTA and application-only flashing
- `version.json` - OTA metadata
- `whatsnew.txt` - What's New text shown by the device
