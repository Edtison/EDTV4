# EDTV4 Calibration and Maintenance

This document collects the calibration and maintenance points that are relevant to the current EDTV4 configuration.

## Calibration sequence

A practical calibration hierarchy is:

1. mechanical inspection,
2. relative nozzle-height alignment,
3. Z-tilt adjustment,
4. probe Z-offset calibration,
5. bed mesh,
6. tool-offset verification,
7. extrusion / pressure-advance verification,
8. first-layer validation.

## Relative nozzle height

Before final Z-offset calibration, the E0 and E1 nozzle tips should be aligned mechanically.

**Target:** no more than approximately **0.02 mm relative Z difference** between the two nozzles.

A dial indicator mounted to the machine frame is the preferred method for checking the relative height. If a nozzle or complete hotend is changed, this check should be repeated.

## Z tilt

The current firmware uses two Z drives and Klipper's `[z_tilt]` function.

Configured probe points:

- `(30, 164)`
- `(470, 164)`

Retry tolerance: `0.05 mm`.

Z tilt should be performed before generating a new bed mesh.

## Probe and Z offset

Current probe configuration:

| Parameter | Value |
|---|---:|
| X offset | 0 mm |
| Y offset | +86 mm |
| Samples | 2 |
| Probe speed | 5 mm/s |
| Retract distance | 2 mm |
| Current saved Z offset | 2.74 mm |

The saved Z offset is machine-specific and can change after mechanical work, nozzle replacement or probe adjustment.

## Bed mesh

Current firmware configuration:

| Parameter | Value |
|---|---:|
| Mesh size | 9 × 9 |
| X range | 30 ... 470 mm |
| Y range | 45 ... 470 mm |
| Algorithm | bicubic |
| Horizontal move Z | 5 mm |
| Mesh speed | 200 mm/s |

The current `SAVE_CONFIG` block contains a stored mesh. The `START_PRINT` macro also requests a fresh `BED_MESH_CALIBRATE`.

## Tool offsets

Current Klipper tool-selection macros apply:

- `T0`: X = 0, Y = 0
- `T1`: X = 0, Y = +0.15 mm

Any mechanical work on the toolheads should be followed by verification of the relative XY tool offset.

## Pressure advance

The machine configuration currently uses a default pressure advance of `0.06` for both extruders. A note in the E0 configuration records the following established values:

- 0.4 mm nozzle: `0.06`
- 0.6 mm nozzle: `0.04`

The tested OrcaSlicer bundles remain the authoritative source for the complete nozzle-specific slicing setup.

## Input shaping

Input shaping is initialized after Klipper startup for both carriages using:

- X: `2hump_ei`, 33 Hz
- Y: `mzv`, 17.5 Hz

The older EDTV3 values present as comments in `printer.cfg` are explicitly marked as reference-only and must not be used for EDTV4.

## Routine inspection

### Before each print

- Check nozzle cleanliness.
- Check the PEI build surface for residue and correct seating.
- Check the nozzle wiping/cleaning area for contamination.
- Confirm that both parked carriages and the motion envelope are unobstructed.

### Approximately every 100 operating hours

- Check X- and Y-belt tension.

### Approximately every 500 operating hours

- Lubricate linear guides and ball screws with a suitable lubricant according to component/manufacturer guidance.
- Inspect 3D-printed machine components for heat, fatigue or mechanical damage.
- Inspect / replace exhaust filtration as required by actual loading.

### Approximately every 2000 operating hours

- Inspect bolted joints and retighten where appropriate.
- Inspect the electrical enclosure for dust or contamination **only after safely isolating the machine from mains power**.

These intervals originate from the original EDTV4 operating documentation and should be adjusted if component suppliers specify shorter service intervals or operating conditions demand more frequent inspection.
