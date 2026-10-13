# EDTV4 Documentation

The public documentation is organized so that current machine configuration and operational information can be read without opening the raw firmware or CAD files first.

## Start here

- [`project_overview.md`](project_overview.md) – machine concept, hardware and source-of-truth hierarchy
- [`system_architecture.md`](system_architecture.md) – control, motion and thermal architecture
- [`operation_quick_reference.md`](operation_quick_reference.md) – concise reference for trained users
- [`calibration_and_maintenance.md`](calibration_and_maintenance.md) – calibration sequence and maintenance intervals
- [`electronics_pinout.md`](electronics_pinout.md) – Octopus Pro connector / MCU pin assignment derived from the current firmware

## Operating manuals

- [Deutsch - EDTV4 Bedienungsanleitung](manuals/EDTV4_Bedienungsanleitung_DE.md)
- [English - EDTV4 Operating Manual](manuals/EDTV4_Operating_Manual_EN.md)

The public manuals are maintained as Markdown so they remain readable directly on GitHub and can be kept in sync with the firmware documentation. Earlier internal PDF versions are intentionally not tracked.

## Related technical sources

- [`../firmware/klipper/`](../firmware/klipper/) – current Klipper configuration and macro reference
- [`../hardware/`](../hardware/) – CAD/manufacturing files and BOM
- [`../slicer/orcaslicer/`](../slicer/orcaslicer/) – tested OrcaSlicer profile bundles
