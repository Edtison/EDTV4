# OrcaSlicer profiles

This directory contains the tested EDTV4 OrcaSlicer printer/profile bundles for:

- 0.4 mm nozzle
- 0.6 mm nozzle
- 1.0 mm nozzle

The profiles are preserved as working machine profiles. Their process parameters, filament profiles and tool-change G-code have **not** been normalized or retuned for this repository.

For publication, only the machine-local `bed_custom_model` path was removed from the printer JSON inside each bundle. This path only referenced a local STL used for the OrcaSlicer bed visualization.

## IDEX placement limitation

When slicing, note that E0 cannot reach the rightmost 40 mm of the print area and E1 cannot reach the leftmost 40 mm. See [`../../media/photos/IDEX_slicing_limits.png`](../../media/photos/IDEX_slicing_limits.png).
