# EDTV4 Project Overview

EDTV4 is a custom large-format **IDEX FFF research platform** developed at the University of Applied Sciences Upper Austria (FH OÖ), Wels. The machine combines mechanical design and manufacturing, a dual-carriage extrusion system, custom electrical integration, Klipper firmware and machine-specific OrcaSlicer profiles.

The repository is intended to serve two purposes:

1. **Technical handover and reuse** for future FH work on the machine.
2. **Engineering documentation** of the completed EDTV4 project.

## System summary

| Area | EDTV4 implementation |
|---|---|
| Process | Fused Filament Fabrication (FFF) |
| Architecture | Enclosed Cartesian IDEX |
| Controller | BIGTREETECH Octopus Pro v1.1, STM32H723 |
| Host | Raspberry Pi 4 |
| Firmware | Klipper |
| User interface | Mainsail / local touchscreen |
| Slicer | OrcaSlicer |
| Extrusion | Two independent E3D Revo Hemera XS based toolheads |
| Bed probing | Z probe with 9 × 9 bed mesh |
| Z system | Dual Z drives with Z-tilt correction |
| Heated enclosure | Active chamber heater and temperature-controlled ventilation |
| Safety / monitoring | Hardware emergency stop, filament switch, filament-motion sensor |

## Firmware-derived machine limits

The current `firmware/klipper/printer.cfg` is the authoritative source for the installed and tested machine configuration.

| Parameter | Current configuration |
|---|---:|
| E0 X travel | -59.5 ... 475 mm |
| E1 X travel | 35 ... 560.29 mm |
| Y travel | -46 ... 505 mm |
| Z travel | -2 ... 650 mm |
| Maximum XY velocity | 300 mm/s |
| Maximum acceleration | 1000 mm/s² |
| Maximum Z velocity | 30 mm/s |
| Maximum Z acceleration | 120 mm/s² |
| E0 configured max. temperature | 310 °C |
| E1 configured max. temperature | 300 °C |
| Heated-bed configured max. temperature | 130 °C |
| Chamber-heater configured max. temperature | 70 °C |
| Bed mesh | 9 × 9, bicubic |

These values are **machine configuration limits**, not general process recommendations.

## IDEX workspace limitation

The two X carriages have different reachable regions due to the physical IDEX arrangement.

- **E0, left carriage:** the rightmost 40 mm of the nominal build area are not reachable.
- **E1, right carriage:** the leftmost 40 mm of the nominal build area are not reachable.

Klipper travel limits prevent a carriage from moving outside its configured mechanical range, but the slicer must still place geometry inside the reachable area of the selected toolhead.

See [`../media/photos/IDEX_slicing_limits.png`](../media/photos/IDEX_slicing_limits.png).

## Mechanical design

The available manufacturing package contains neutral CAD and production drawings for major EDTV4 components. Depending on the component, the repository includes STEP, DWG and PDF files.

Main groups include:

- sheet-metal frame and enclosure parts,
- acrylic enclosure panels,
- X-axis carriage and extruder mounting parts,
- belt tensioners and motion-system brackets,
- electrical enclosure components,
- the final neutral STEP assembly.

See [`../hardware/cad/`](../hardware/cad/).

## Motion system

EDTV4 uses a Cartesian architecture with two independent X carriages.

- Two X drives: one per carriage.
- Two Y motors.
- Two Z drives.
- Z homing uses the probe as a virtual Z endstop.
- Z-tilt correction aligns the two Z sides before bed meshing.
- A 9 × 9 bed mesh compensates residual build-plate deviation.

The current motion limits, driver allocations and I/O mapping are documented in [`electronics_pinout.md`](electronics_pinout.md).

## Extrusion and tool changing

Two independent toolheads are defined in Klipper as `extruder` / E0 and `extruder1` / E1. Tool selection uses the `T0` and `T1` macros together with Klipper's `dual_carriage` functionality.

The tested OrcaSlicer bundles contain the process and tool-change routines used on the machine. They are intentionally distributed as tested rather than normalized across nozzle sizes.

See [`../slicer/orcaslicer/`](../slicer/orcaslicer/).

## Thermal system

The current firmware defines four controlled heating functions:

- E0 hotend,
- E1 hotend,
- heated bed,
- actively heated chamber.

The chamber also uses a temperature-controlled fan and a recirculation fan. The recirculation fan starts at 25% after Klipper initialization and can be adjusted through dedicated macros.

## Firmware and control stack

The machine runs Klipper on a BIGTREETECH Octopus Pro v1.1 connected to a Raspberry Pi host. The repository contains the current machine-specific `printer.cfg`.

Notable firmware functions include:

- IDEX carriage selection and parking,
- custom homing sequence,
- Z-tilt adjustment,
- automatic bed mesh calibration,
- chamber temperature commands (`M141`, `M191`),
- filament runout / motion monitoring,
- hardware emergency-stop input,
- recirculation-fan control,
- per-carriage input-shaper initialization.

See [`../firmware/klipper/README.md`](../firmware/klipper/README.md) and [`../firmware/klipper/macros.md`](../firmware/klipper/macros.md).

## Source-of-truth hierarchy

When documents disagree, use the following priority:

1. **Current `printer.cfg`** for installed firmware configuration and machine limits.
2. **Current tested OrcaSlicer profile bundle** for slicing and tool-change behaviour.
3. **CAD / manufacturing files** for geometry and production information.
4. **Public operating manuals** for trained-user operating procedures.

Earlier internal operating documentation may contain superseded machine values and must not be used to override the current firmware configuration. The public manuals in `docs/manuals/` have been rebuilt against the current repository baseline.
