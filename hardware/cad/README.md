# CAD and manufacturing files

This directory contains the available EDTV4 manufacturing drawings and neutral CAD exports.

## Assembly

`EDTV4_Assembly_STEP.zip` contains the final neutral STEP assembly export:

- File: `EDTV4_Assembly_2026-10-13.stp`
- STEP schema: AP214 / `AUTOMOTIVE_DESIGN`
- Originating CAD system: Autodesk Inventor 2020

The STEP header was cleaned for publication so that no local Windows/OneDrive path is embedded in the public file. The geometry was not modified. The same metadata cleanup was applied to the individual STEP manufacturing exports while leaving each STEP `DATA` section unchanged.

## Manufacturing data

The subdirectories contain available DWG, PDF and STEP data for:

- `acrylics/`
- `electric_enclosure/`
- `milled_parts/`
- `sheetmetal/`

File names containing escaped Unicode sequences from the original export were corrected for readability. One byte-identical duplicate STEP file was removed from `sheetmetal/`; the canonical copy remains in `electric_enclosure/`.
