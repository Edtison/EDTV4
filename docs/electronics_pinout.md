# EDTV4 Electronics / Pinout

This document is derived from the current `firmware/klipper/printer.cfg` in this repository.
The MCU pin-to-connector mapping was cross-checked against the Klipper generic configuration for the **BIGTREETECH Octopus Pro v1.1** and the official BIGTREETECH documentation.

> **Important:** This is a documentation snapshot of the current EDTV4 configuration. If `printer.cfg` changes, this document must be updated as well. Do not use it as a generic Octopus Pro wiring guide.

## Controller

- Mainboard: BIGTREETECH Octopus Pro v1.1
- MCU: STM32H723
- Firmware: Klipper
- Host connection: USB
- Klipper MCU serial currently configured in `printer.cfg`

## Stepper driver allocation

| EDTV4 function | Octopus socket | Step | Direction | Enable | Driver communication | Driver settings in firmware |
|---|---|---|---|---|---|---|
| Extruder E0 | MOTOR0 / Driver0 | `PF13` | `!PF12` | `!PF14` | TMC2209 UART `PC4` | 0.700 A run / 0.500 A hold |
| Extruder E1 | MOTOR1 / Driver1 | `PG0` | `!PG1` | `!PF15` | TMC2209 UART `PD11` | 0.700 A run / 0.500 A hold |
| Y motor 2 (`stepper_y1`) | MOTOR2 / Driver2 | `PF11` | `PG3` | `!PG5` | TMC5160 SPI1, CS `PC6` | 0.800 A run / 0.500 A hold |
| Y motor 1 (`stepper_y`) | MOTOR3 / Driver3 | `PG4` | `!PC1` | `!PA2` | TMC5160 SPI1, CS `PC7` | 0.800 A run / 0.500 A hold |
| Right X carriage / E1 (`dual_carriage`) | MOTOR4 / Driver4 | `PF9` | `PF10` | `!PG2` | TMC5160 SPI1, CS `PF2` | 0.800 A run / 0.500 A hold |
| Left X carriage / E0 (`stepper_x`) | MOTOR5 / Driver5 | `PC13` | `PF0` | `!PF1` | TMC5160 SPI1, CS `PE4` | 0.800 A run / 0.500 A hold |
| Z motor 2 (`stepper_z1`) | MOTOR6 / Driver6 | `PE2` | `PE3` | `!PD4` | Not declared in `printer.cfg` | - |
| Z motor 1 (`stepper_z`) | MOTOR7 / Driver7 | `PE6` | `PA14` | `!PE0` | Not declared in `printer.cfg` | - |

Klipper pin modifiers: `!` means inverted logic and `^` enables the MCU pull-up.

## Endstops, probe and filament sensors

| Function | Octopus input | MCU pin | Klipper configuration |
|---|---|---|---|
| Hardware emergency stop | STOP0 | `PG6` | `[gcode_button Hardware_Notaus]` |
| Unused | STOP1 | `PG9` | - |
| Filament switch sensor | STOP2 | `^PG10` | `[filament_switch_sensor switch_sensor]` |
| Filament motion encoder | STOP3 | `^PG11` | `[filament_motion_sensor encoder_sensor]` |
| Y motor 2 endstop | STOP4 | `^PG12` | `stepper_y1` |
| Y motor 1 endstop | STOP5 | `^PG13` | `stepper_y` |
| Right carriage E1 endstop | STOP6 | `^PG14` | `dual_carriage` |
| Left carriage E0 endstop | STOP7 | `^PG15` | `stepper_x` |
| Z probe | PROBE | `!PC5` | `[probe]` |

The Z axis uses `probe:z_virtual_endstop`; no dedicated Z endstop switch is configured.

## Heaters and temperature sensors

| Function | Heater output | Heater pin | Temperature input | Sensor pin | Sensor type | Configured max. temperature |
|---|---|---|---|---|---|---:|
| Extruder E0 | HE0 | `PA0` | T1 | `PF5` | ATC Semitec 104GT-2 | 310 °C |
| Extruder E1 | HE1 | `PA3` | TB | `PF3` | ATC Semitec 104GT-2 | 300 °C |
| Chamber heater | HE2 | `PB0` | T0 | `PF4` | Generic 3950 | 70 °C |
| Heated bed | HE3 | `PB11` | T2 | `PF6` | NTC 100K MGB18-104F39050L32 | 130 °C |

**Notable wiring detail:** the current EDTV4 configuration uses **HE3 (`PB11`) for the heated bed**. The dedicated Octopus heated-bed MOSFET output (`PA1`) is not used by the current `printer.cfg`.

The chamber temperature sensor on `PF4` is shared by the chamber heater and the chamber temperature-controlled fan via `[duplicate_pin_override]`.

## Fans

| Function | Octopus output | MCU pin | Klipper section / note |
|---|---|---|---|
| E0 hotend fan | FAN0 | `PA8` | `[heater_fan fan1]` |
| Part cooling output A | FAN1 | `PE5` | Combined through `[multi_pin pinfan]` |
| E1 hotend fan | FAN2 | `PD12` | `[heater_fan fan2]` |
| Part cooling output B | FAN3 | `PD13` | Combined through `[multi_pin pinfan]` |
| Chamber temperature fan | FAN4 | `PD14` | `[temperature_fan Innenraum]` |
| Recirculation fan | FAN5 | `PD15` | `[fan_generic recirc_fan]`, PWM at ~20 kHz |

The two part-cooling outputs (`PE5` and `PD13`) are driven together by the standard Klipper `[fan]` object.

## Firmware-derived motion limits

These are firmware limits, not a statement of the universally usable print area.

| Axis / carriage | Configured range |
|---|---:|
| Left X carriage E0 | -59.5 ... 475 mm |
| Right X carriage E1 | 35 ... 560.29 mm |
| Y | -46 ... 505 mm |
| Z | -2 ... 650 mm |
| Maximum XY velocity | 300 mm/s |
| Maximum acceleration | 1000 mm/s² |
| Maximum Z velocity | 30 mm/s |
| Maximum Z acceleration | 120 mm/s² |

For actual model placement, also observe the IDEX side-area restriction documented in `media/photos/IDEX_slicing_limits.png`.

## Bed probing / mesh

- Probe offset: X = 0 mm, Y = +86 mm
- Bed mesh: 9 × 9 points
- Mesh area: X 30 ... 470 mm, Y 45 ... 470 mm
- Algorithm: bicubic
- Z tilt reference positions: two Z drives

## Reference mapping

- Klipper generic Octopus Pro v1.1 configuration: https://github.com/Klipper3d/klipper/blob/master/config/generic-bigtreetech-octopus-pro-v1.1.cfg
- BIGTREETECH Octopus Pro repository: https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro

