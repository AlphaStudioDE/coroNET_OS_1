# coroNET OS 1 Bill of Materials

This BOM is a living hardware reference for coroNET OS 1 builds.

The exact part numbers may change between prototype, pre-production, and product revisions. Use this file as the project-level hardware checklist, then lock part numbers per hardware revision before manufacturing or publishing a public build guide.

> Status: draft BOM overview for the stable 1.6.x firmware line.

---

## Core Electronics

| Item | Qty | Notes |
| --- | ---: | --- |
| JC3248W535 ESP32-S3 touchscreen controller | 1 | Main UI, WiFi, firmware, LED/audio/control logic |
| Touch display assembly | 1 | Integrated with the JC3248W535 controller |
| microSD card, 500 MB to 2 GB | 1 | A 500 MB card should be included with the ESP32-S3 display kit. If it is missing, buy a 500 MB to 2 GB card; larger cards may cause read issues on some ESP32-S3 setups. Stores sound files such as `boot.wav` and optional `license.txt` |
| microSD interface | 1 | Built into controller board or external module, depending on hardware revision |

---

## LED System

| Item | Qty | Notes |
| --- | ---: | --- |
| SK6812 RGBNW LED strip, 1 m / 60 LEDs / IP20 | 1 | 42 outer LEDs + 18 inside LEDs |
| LED wiring / strip / PCB | 1 set | Must follow the configured physical LED order |
| LED power wiring | 1 set | Size for expected LED current and voltage drop |
| LED data wiring | 1 set | Connected to the firmware LED data pin |

Current firmware LED layout:

- `0..10` right outer section
- `11..30` center/front outer section
- `31..41` left outer section
- `42..59` inside section

Mirror LED mode can be enabled in firmware for units wired in the previous direction.

---

## Audio System

| Item | Qty | Notes |
| --- | ---: | --- |
| I2S audio amplifier | 1 | Used for boot sound and status/event feedback |
| Speaker, 4 ohm / 3 W / 1.25 mm connector | 1 | Match amplifier output and enclosure acoustics |
| Audio wiring | 1 set | I2S signal, power, and speaker wiring |

Recommended SD root file:

- `boot.wav` - startup audio for the boot animation

SD card note:

- Use the included 500 MB microSD card where possible. If the kit does not include one, use a 500 MB to 2 GB card. Avoid larger cards unless they have been tested with the target ESP32-S3 board.

---

## Ventilation / Motion

| Item | Qty | Notes |
| --- | ---: | --- |
| PWM fan | 1 | Controlled by firmware fan PWM output |
| Servo | 1 | Drives the ventilation flap; exact model depends on the mechanical revision |
| Ventilation flap mechanism | 1 | Mechanical assembly controlled by the servo |
| Fan duct / airflow path | 1 | Depends on enclosure design |

Before final assembly, verify:

- fan direction
- fan minimum and maximum PWM behavior
- servo open/closed positions
- failsafe flap and fan values

---

## Temperature And Printer Telemetry

| Item | Qty | Notes |
| --- | ---: | --- |
| Printer telemetry connection | 1 | Moonraker-style network telemetry used by firmware |
| Chamber temperature source | 1 | Usually read from printer telemetry rather than a separate local sensor |

Optional or revision-dependent:

- local chamber temperature sensor
- external fan/temperature controller integration
- additional heater or dry-box related hardware

---

## Power

| Item | Qty | Notes |
| --- | ---: | --- |
| Main power input | 1 | Must match controller, LEDs, audio, fan, and servo requirements |
| 5 V / 10 A power supply or regulator | 1 | Required for LEDs and peripheral load |
| 10 A fuse / inline protection component | 0-1 | Optional safety protection; exact holder and wiring path depend on the build |
| 1000 uF / 10 V capacitor | 1+ | Recommended near LED power input to reduce voltage dips |
| Power distribution wiring or PCB | 1 | Size for LED current and peripheral load |
| Common ground wiring | 1 set | Required between controller, LEDs, audio, fan, and servo |

Power notes:

- Size LED power for worst-case RGBW load.
- Avoid powering high-current LEDs through thin controller traces.
- Keep audio, LED, servo, and fan power stable during boot and OTA.
- Use proper fusing or protection in product hardware.
- The 10 A fuse is an optional protection element. Skipping it can reduce the build cost, but the builder accepts that risk and is responsible for safe wiring, insulation, and current protection.

---

## Mechanical Parts

| Item | Qty | Notes |
| --- | ---: | --- |
| coroNET OS 1 enclosure | 1 | Printable housing for display, LEDs, audio, and ventilation; use [coroNET.3mf](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf) |
| LED diffuser / light guide | 1 set | Required for polished visual output |
| M2.5 screw / spacer / insert kit | 1 set | 631-piece kit used for display/enclosure assembly |
| Display mounting hardware | 1 set | Screws, brackets, inserts, or adhesive system |
| Speaker mounting hardware | 1 set | Depends on enclosure design |
| Fan and flap mounting hardware | 1 set | Depends on ventilation design |
| Cable management parts | 1 set | Strain relief, clips, sleeves, or channels |

Printable mechanical parts and print settings are provided in [coroNET.3mf](https://github.com/AlphaStudioDE/coroNET_OS_1/raw/main/coroNET.3mf).

---

## Wiring And Connectors

| Item | Qty | Notes |
| --- | ---: | --- |
| LED connector | 1+ | Power, ground, and data |
| Fan connector | 1 | Power, ground, PWM/tach if used |
| Servo connector | 1 | Power, ground, signal |
| Speaker connector | 1 | Optional but recommended for serviceability |
| Power connector | 1 | Match current requirements |
| Solderless DC connector set | 1 set | Optional male/female DC 5.5 x 2.1/2.5 mm connector set for cleaner power wiring |
| DC barrel extension cable, 5.5 x 2.5 mm, male to female, 20 cm | 1 | Power input or service extension cable |
| FFC/FPC cable, 8P, same direction, 15-20 cm | 1 | Display/controller connection, exact role to confirm |
| FFC/FPC cable, 4P, same direction, 20-30 cm | 1 | Touch/peripheral connection, exact role to confirm |
| 24 AWG to 28 AWG wire set | 1 set | Internal wiring and signal connections |
| Internal wiring harness | 1 set | Final lengths depend on enclosure design |

---

## Optional / Revision-Dependent Parts

| Item | Qty | Notes |
| --- | ---: | --- |
| External activation/license file | 1 | `license.txt` on SD card where required |
| Additional temperature sensor | 0-1 | If not relying only on printer telemetry |
| Additional fan controller hardware | 0-1 | Depending on hardware revision |
| Custom PCB | 0-1 | Recommended for product builds |
| Debug/programming connector | 0-1 | Useful for service and firmware recovery |
| Wire terminal / crimp connector kit | 0-1 | Optional build aid; can be replaced by direct soldering |
| Connector / cable joint kit | 0-1 | Optional build aid for cleaner internal wiring |
| Inline splice / quick cable connector set | 0-1 | Optional build aid; use only if current rating and mechanical retention are suitable |

Cost notes:

- Some common workshop parts may already be owned by the builder, such as screws, wires, connectors, small electronics, or transistors. Reusing suitable parts can reduce the actual build cost.
- Optional connector and cable-joint kits can make assembly easier and cleaner, but they are not mandatory. The same electrical connections can be made by properly soldering and insulating the wires.

---

## Current Sourced Parts

These links reflect the current prototype sourcing list. Product builds should verify supplier stability, exact variants, voltage/current ratings, lead time, and available alternatives before public release.

Prices below are a snapshot for one unit of each listed line item and may change by supplier, region, shipping option, VAT handling, coupon, or selected variant.

| Category | Part / Variant | Qty | Unit price | Source |
| --- | --- | ---: | ---: | --- |
| Power cable | DC barrel extension, 5.5 x 2.5 mm, male to female, 20 cm | 1 | 1.79 EUR | [AliExpress item 1005010208980172](https://de.aliexpress.com/item/1005010208980172.html) |
| Power supply | 5 V / 10 A | 1 | 11.99 EUR | [AliExpress item 1005004121728138](https://de.aliexpress.com/item/1005004121728138.html) |
| Controller/display | JC3248W535 | 1 | 21.99 EUR | [AliExpress item 1005007566315926](https://de.aliexpress.com/item/1005007566315926.html) |
| LEDs | SK6812 RGBNW, 1 m / 60 LEDs / IP20 | 1 | 5.29 EUR | [AliExpress item 1005005824057524](https://de.aliexpress.com/item/1005005824057524.html) |
| Speaker | 4 ohm / 3 W / 1.25 mm connector | 1 | 1.69 EUR | [AliExpress item 1005008267900755](https://de.aliexpress.com/item/1005008267900755.html) |
| Servo | Micro servo for ventilation flap control | 1 | 1.95 EUR | [AliExpress item 1005008315780030](https://de.aliexpress.com/item/1005008315780030.html) |
| Wire | 24 AWG to 28 AWG wire set | 1 set | 8.99 EUR | [AliExpress item 1005007670937847](https://de.aliexpress.com/item/1005007670937847.html) |
| Power filtering | 1000 uF / 10 V capacitor | 1+ | 2.55 EUR | [AliExpress item 1005002075527957](https://de.aliexpress.com/item/1005002075527957.html) |
| Optional protection | 10 A fuse / inline protection component | 1 | 2.05 EUR | [AliExpress item 1005009895179310](https://de.aliexpress.com/item/1005009895179310.html) |
| Hardware kit | M2.5 screw / spacer kit, 631 pieces | 1 set | 7.69 EUR | [AliExpress item 1005009682333826](https://de.aliexpress.com/item/1005009682333826.html) |
| Power connector | Solderless DC 5.5 x 2.1/2.5 mm male/female connector set, 5 pairs | 1 set | 2.49 EUR | [AliExpress item 1005008713574522](https://de.aliexpress.com/item/1005008713574522.html) |
| FFC/FPC cable | 8P, same direction, 15-20 cm | 1 | 4.79 EUR | [AliExpress item 1005004462513465](https://de.aliexpress.com/item/1005004462513465.html) |
| FFC/FPC cable | 4P, same direction, 20-30 cm | 1 | 4.19 EUR | [AliExpress item 1005004462513465](https://de.aliexpress.com/item/1005004462513465.html) |

Estimated sourced-parts subtotal: 77.45 EUR, excluding shipping and any optional/revision-dependent parts not listed in this table.

Estimated subtotal without the optional 10 A fuse/protection component: 75.40 EUR.

Some items in this table, especially screws, wire, connectors, and generic small electronics, may already be available in a builder's workshop. In that case, the real build cost can be lower than the listed sourced-parts subtotal.

### Optional Build-Aid Links

These products are not required for the firmware or the base hardware design. They are example purchases that can make cable routing, clamping, or serviceable connections easier. They may be replaced by direct soldering with proper insulation and strain relief.

| Category | Purpose | Source |
| --- | --- | --- |
| Cable terminals / connector kit | Optional cable clamping and connector assortment | [AliExpress item 1005006963063019](https://de.aliexpress.com/item/1005006963063019.html) |
| Connector / crimp kit | Optional connector assortment for cleaner wiring | [AliExpress item 1005005961638278](https://de.aliexpress.com/item/1005005961638278.html) |
| Inline cable connectors | Optional quick cable joints or wire splices | [AliExpress item 1005008599216565](https://de.aliexpress.com/item/1005008599216565.html) |

---

## Information Still To Lock

Before publishing a final public BOM, fill in:

- exact audio amplifier model
- exact fan model, voltage, and current
- final servo torque and mechanical throw under load
- final 10 A fuse holder, wiring path, and enclosure mounting method
- final connector types
- final wire gauges per current path
- enclosure revision
- diffuser/light-guide material
- estimated cost per unit

---

## Revision Notes

| Hardware revision | Firmware line | Notes |
| --- | --- | --- |
| Prototype / wired build | 1.6.x | Active development and validation hardware |
| Pre-production | TBD | To be defined before Kickstarter/public build guide |
| Production | TBD | To be defined after mechanical and sourcing lock |
