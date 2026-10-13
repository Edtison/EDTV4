# EDTV4 Klipper Macro Reference

This page describes the custom macros defined in the current `printer.cfg`. It is documentation only; the source of truth remains [`printer.cfg`](printer.cfg).

## Tool selection and parking

| Macro | Purpose |
|---|---|
| `T0` | Select left carriage / E0, activate `extruder`, apply zero XY offset |
| `T1` | Select right carriage / E1, activate `extruder1`, apply Y offset +0.15 mm |
| `PARK_extruder` | Park E0 at approximately X = -57 mm |
| `PARK_extruder1` | Park E1 at approximately X = 557 mm |

## Homing and print start

| Macro | Purpose |
|---|---|
| `Retract_X` | Creates a controlled pre-homing separation movement for the two X carriages |
| `Homing` | Runs the custom EDTV4 homing sequence and parks both carriages after `G28` |
| `START_PRINT` | Homing → Z tilt → bed mesh calibration → move to Z = 5 mm |
| `END_PRINT` | Turns off bed/hotend/fan, retracts and parks the active tool |

## Extrusion helper macros

| Macro | Purpose |
|---|---|
| `REFILL` | Relative positive extrusion while preserving/restoring G-code state |
| `RETRACT` | Relative negative extrusion while preserving/restoring G-code state |

The tested OrcaSlicer profiles call these helpers as part of their machine-specific tool-change behaviour.

## Chamber control

| Macro / command | Purpose |
|---|---|
| `M141 S<temp>` | Set chamber heater target without waiting |
| `M191 S<temp>` | Set chamber target and wait until the requested minimum temperature is reached |
| `COOLDOWN` | Set E0, E1 and heated-bed targets to 0 °C |

The chamber heater is configured as `heater_generic chamber` on `PB0` and uses the sensor on `PF4`.

## Recirculation fan

| Macro | Purpose |
|---|---|
| `RECIRC_ON S=<0..1>` | Set recirculation fan speed; default 25% |
| `RECIRC_OFF` | Stop recirculation fan |
| `RECIRC_INC STEP=<value>` | Increase speed, default +25 percentage points |
| `RECIRC_DEC STEP=<value>` | Decrease speed, default -25 percentage points |
| `RECIRC_BOOT` | Starts the recirculation fan at 25% after Klipper initialization |

## Z service macro

`SET_Z0` uses `SET_KINEMATIC_POSITION Z=0`. It is a service/maintenance aid and must only be used when the operator understands the actual physical Z position of the machine.

## Hardware emergency stop

The `[gcode_button Hardware_Notaus]` input calls Klipper's emergency-stop action when the hardware input on `PG6` is triggered.
