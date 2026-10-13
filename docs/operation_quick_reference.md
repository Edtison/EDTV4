# EDTV4 Operation Quick Reference

> **Scope:** This page is a concise public reference for trained users. It does not replace machine-specific instruction, electrical safety procedures or the final operating manual.

## Before switching on

- Perform a visual check of the machine and build area.
- Confirm that no tools or loose parts are inside the motion envelope.
- Check the PEI build surface and both nozzles for contamination.
- Ensure the hardware emergency stop is released before starting the machine. Starting Klipper with the emergency-stop input active can prevent normal initialization.

## Start-up

1. Release the emergency stop if it is engaged.
2. Switch on the machine using the main power switch.
3. Allow the Raspberry Pi / Klipper system to initialize.
4. Check the Mainsail interface for configuration or MCU errors before moving the machine.

## Shutdown

1. Finish or cancel the active print.
2. Allow hot components to cool as required.
3. Shut down the host system through the Klipper/Mainsail user interface.
4. Only then switch off the main power.

Do not routinely remove mains power while the host operating system is still running.

## Filament change

Filament loading/unloading is performed manually.

1. Select the required extruder.
2. Heat the hotend to a temperature suitable for the material being removed and inserted.
3. Remove the old filament.
4. Insert and engage the new filament in the extruder.
5. Extrude enough material to fully purge the old polymer / colour. The original operating procedure used at least approximately 150 mm at 5 mm/s as a starting point for a complete change.

## Nozzle / toolhead height alignment

The two nozzle tips must be mechanically aligned in Z before performing the final probe Z-offset calibration.

- Target maximum nozzle-to-nozzle Z deviation: **0.02 mm**.
- A dial indicator is recommended for mechanical comparison.
- If a nozzle is changed, re-check the relative height of both toolheads before printing.

## Z-offset calibration

After confirming the mechanical nozzle alignment:

1. Home the machine.
2. Start the Klipper probe/Z calibration procedure.
3. Adjust the nozzle-to-bed distance using a paper-feel method or the laboratory's preferred reference method.
4. Save the new offset.
5. Verify the first layer before a production print.

The current firmware uses the probe as the virtual Z endstop and stores the calibrated probe offset in the Klipper `SAVE_CONFIG` block.

## Starting a print

Use one of the tested EDTV4 OrcaSlicer profiles from [`../slicer/orcaslicer/`](../slicer/orcaslicer/).

Before slicing, observe the IDEX reach limitation:

- E0 cannot reach the rightmost 40 mm.
- E1 cannot reach the leftmost 40 mm.

The start sequence defined in the current firmware performs homing, Z-tilt adjustment and bed-mesh calibration before printing.

## Low-temperature materials

The original operating procedure requires additional enclosure ventilation when printing materials with a glass-transition temperature below approximately 70 °C. For PLA/PETG-type operation, the door was kept at least about 5 cm open and the upper acrylic flap fully open to reduce the risk of excessive cold-end temperature and heat creep.

Treat this as a machine-specific operating constraint and verify the actual hotend/cold-end behaviour when changing toolhead hardware or enclosure airflow.

## Emergency stop

A hardware emergency-stop input is connected to `STOP0 / PG6` and triggers Klipper's emergency-stop action.

After an emergency stop:

- identify and correct the cause before restart,
- inspect the machine for mechanical or thermal damage,
- restart Klipper / the machine only when the system is safe to operate.

## Full operating manuals

For the complete public operating procedure, see:

- [Deutsch - EDTV4 Bedienungsanleitung](manuals/EDTV4_Bedienungsanleitung_DE.md)
- [English - EDTV4 Operating Manual](manuals/EDTV4_Operating_Manual_EN.md)
