# EDTV4 Operating Manual

**Public revision - 13 October 2026**

EDTV4 is a large-format enclosed IDEX FFF research platform at the University of Applied Sciences Upper Austria (FH OÖ), Wels. This manual describes basic operation of the completed machine and does not replace machine-specific training.

> **Important:** the current [`printer.cfg`](../../firmware/klipper/printer.cfg) is authoritative for installed firmware values and configured machine limits. The limits shown here are not general material or process recommendations.

## 1. System overview

| Area | Current implementation |
|---|---|
| Process | FFF / IDEX |
| Controller | BIGTREETECH Octopus Pro v1.1 |
| Host | Raspberry Pi 4 |
| Firmware | Klipper |
| User interface | Mainsail / local touchscreen |
| Slicer | OrcaSlicer |
| X system | Two independent X carriages |
| Y system | Two Y motors |
| Z system | Two Z drives with Z-tilt correction |
| Bed probing | 9 × 9, bicubic |
| Filament monitoring | Filament switch + motion sensor |
| Safety function | Hardware emergency stop |

### 1.1 Configured firmware limits

| Parameter | Configured value |
|---|---:|
| E0 X travel | -59.5 ... 475 mm |
| E1 X travel | 35 ... 560.29 mm |
| Y travel | -46 ... 505 mm |
| Z travel | -2 ... 650 mm |
| Maximum XY velocity | 300 mm/s |
| Maximum acceleration | 1000 mm/s² |
| E0 configured maximum temperature | 310 °C |
| E1 configured maximum temperature | 300 °C |
| Heated-bed configured maximum temperature | 130 °C |
| Chamber-heater configured maximum temperature | 70 °C |

Actual print temperatures and process parameters depend on material, nozzle and profile. Use the tested EDTV4 OrcaSlicer profiles for machine operation.

## 2. Before powering on

Before each start:

1. Check the build chamber and motion envelope for loose parts or tools.
2. Check that the PEI build surface is clean and correctly seated.
3. Check both nozzles for filament residue or heavy contamination.
4. Inspect the wiper/cleaning area.
5. Ensure that both X carriages can move freely.
6. Confirm that the hardware emergency stop is **released**.

Starting Klipper while the emergency-stop input is active can prevent normal initialization. After correcting the cause, restart Klipper or the machine as required.

## 3. Powering on

1. Release the emergency stop if the machine is in a safe condition to start.
2. Switch on the main power.
3. Allow the Raspberry Pi and Klipper to initialize completely.
4. Open Mainsail or the local touchscreen interface.
5. Before moving the machine, verify that Klipper, the MCU, sensors and heaters report no errors.

Do not move axes manually while a configuration, MCU or sensor fault is active.

## 4. Powering off

1. Finish or cancel any active print correctly.
2. Switch off heaters and allow hot components to cool as required.
3. Shut down the host system through the Klipper/Mainsail interface.
4. Only then switch off the main power.

Do not routinely shut down the Raspberry Pi by directly removing mains power.

## 5. Changing filament

Filament loading and unloading are performed manually.

1. Select the required extruder.
2. Heat the hotend to a temperature suitable for both the material being removed and the material being loaded. When changing materials, ensure that the higher-melting polymer can be processed safely.
3. Remove the old filament.
4. Insert the new filament and engage it in the extruder.
5. Extrude until the previous polymer or colour has been fully purged.

An established EDTV4 starting point for a complete material change was at least approximately **150 mm at 5 mm/s**. Purge further for strong colour or material changes.

## 6. Nozzle height and relative toolhead alignment

Before final Z-offset calibration, the E0 and E1 nozzle tips must be mechanically aligned in Z.

**Target:** no more than approximately **0.02 mm relative height difference**.

A dial indicator mounted to the machine frame is recommended. Repeat this check after changing a nozzle or hotend.

## 7. Calibrating the Z offset

The current firmware uses the probe as the virtual Z endstop.

1. Confirm the mechanical nozzle alignment described in section 6.
2. Home the machine.
3. Start the Klipper probe/Z calibration procedure.
4. Set the nozzle-to-bed distance using a paper-feel method or the laboratory's approved reference method.
5. Save the value.
6. After restart, perform a first-layer verification.

The stored probe Z offset is machine-specific and must be rechecked after relevant mechanical changes.

More details: [`../calibration_and_maintenance.md`](../calibration_and_maintenance.md).

## 8. Changing a nozzle

1. Heat the affected extruder to a temperature suitable for the loaded filament and unload the filament completely.
2. Allow the hotend to cool completely to a safe nozzle-change temperature.
3. Change the nozzle according to the installed Revo system procedure.
4. Check that the new nozzle/hotend is seated correctly.
5. Re-align the relative E0/E1 nozzle height to within approximately 0.02 mm.
6. Recheck Z offset and first-layer behaviour.

If there is any uncertainty, follow the manufacturer's instructions for the installed hotend/nozzle system.

## 9. Preparing and starting a print

### 9.1 OrcaSlicer profile

Use one of the tested EDTV4 profiles in [`../../slicer/orcaslicer/`](../../slicer/orcaslicer/). The provided 0.4 mm, 0.6 mm and 1.0 mm profiles are preserved as working machine states and intentionally contain some different tool-change and process parameters.

### 9.2 Observe the IDEX workspace limitation

The two X carriages do not reach the same part of the nominal print area:

- **E0 / left extruder:** the rightmost **40 mm** are not reachable.
- **E1 / right extruder:** the leftmost **40 mm** are not reachable.

Klipper limits the mechanical travel, but model placement in the slicer must still respect the reachable area of the selected toolhead.

![IDEX workspace limitation](../../media/photos/IDEX_slicing_limits.png)

### 9.3 Connecting to the printer

EDTV4 is accessed through the network connection configured at its installation location and through Klipper/Mainsail. Network addresses and credentials are **not documented publicly** and must be obtained from the responsible laboratory staff.

In OrcaSlicer, EDTV4 can be configured as a physical Klipper printer using the valid local address.

### 9.4 Automatic start sequence

The current `START_PRINT` sequence performs:

1. custom homing,
2. `Z_TILT_ADJUST`,
3. a new `BED_MESH_CALIBRATE`,
4. movement to Z = 5 mm.

The complete motion and probing area must therefore be clear before starting a print.

## 10. PLA, PETG and other lower-temperature materials

For materials with a glass-transition temperature below approximately **70 °C**, the established EDTV4 operating procedure is intended to reduce cold-end heat buildup and heat creep:

- keep the front door open by at least approximately **5 cm**,
- keep the upper acrylic flap fully open.

If toolhead hardware, fan configuration or enclosure airflow is changed, verify the actual thermal behaviour again.

## 11. Heated chamber

The current firmware configures the active chamber heater up to a maximum of **70 °C**. Chamber control is available through the machine macros or a suitable tested slicer profile.

- `M141 S<temperature>`: set the chamber target without waiting.
- `M191 S<temperature>`: set the chamber target and wait until the requested minimum temperature is reached.

Do not use target temperatures above values approved for the machine, material and installed components.

## 12. Emergency stop

The hardware emergency stop is connected in the current firmware as a safety input on `STOP0 / PG6` and triggers Klipper's emergency-stop function.

After an emergency stop:

1. identify and correct the cause,
2. inspect the machine for mechanical, electrical and thermal damage,
3. release the emergency stop only after a safe condition has been restored,
4. restart Klipper/the machine and verify correct operation.

The emergency stop is not a normal power-off control.

## 13. Maintenance

### Before each print

- Check and clean the PEI surface.
- Check the nozzles for residue.
- Inspect the wiper/cleaning area.
- Keep the motion envelope and carriage parking positions clear.

### Approximately every 100 operating hours

- Check X- and Y-belt tension.

### Approximately every 500 operating hours

- Lubricate linear guides and ball screws according to component/manufacturer guidance.
- Inspect installed 3D-printed machine components for heat, fatigue or mechanical damage.
- Inspect/replace exhaust filtration according to actual loading.

### Approximately every 2000 operating hours

- Inspect bolted joints and retighten where appropriate.
- Inspect the electrical enclosure for dust and contamination. **Safely isolate the machine from mains power before work inside the electrical enclosure.**

These intervals originate from the established EDTV4 operating documentation and should be adjusted if component suppliers specify shorter intervals or actual operating conditions require more frequent service.

## 14. Calibration after modifications

The following changes require at least partial recalibration:

| Change | Required check |
|---|---|
| Nozzle changed | Nozzle height, Z offset, first layer |
| Hotend/extruder mechanically loosened | Nozzle height, tool offset, Z offset |
| Probe changed or moved | Probe Z offset, bed mesh |
| Z mechanics changed | Z tilt, Z offset, bed mesh |
| X/Y mechanics changed | Homing, tool offset, input shaping check |
| Slicer/tool-change change | Controlled test print first |

Detailed values: [`../calibration_and_maintenance.md`](../calibration_and_maintenance.md).

## 15. Technical documentation

- [Project overview](../project_overview.md)
- [System architecture](../system_architecture.md)
- [Electronics / pinout](../electronics_pinout.md)
- [Calibration & maintenance](../calibration_and_maintenance.md)
- [Klipper configuration](../../firmware/klipper/README.md)
- [Klipper macro reference](../../firmware/klipper/macros.md)
- [OrcaSlicer profiles](../../slicer/orcaslicer/README.md)
- [CAD and manufacturing data](../../hardware/cad/README.md)
- [Public bill of materials](../../hardware/bom/README.md)

## 16. Document status

This public revision was rebuilt from the original EDTV4 operating documentation and the current repository. Internal network information, personal student identifiers and superseded technical limits were intentionally omitted.
