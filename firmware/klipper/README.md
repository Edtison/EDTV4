# Klipper Configuration

`printer.cfg` contains the current EDTV4 machine configuration and is the primary technical reference for configured motion limits, heaters, sensors, I/O assignments and custom macros.

## Important

This file is **machine-specific**. It is not a generic Octopus Pro configuration and should not be copied to another printer without checking the complete wiring and hardware first.

The configuration expects `mainsail.cfg` to be available on the Klipper installation through:

```ini
[include mainsail.cfg]
```

## Documentation

- [`printer.cfg`](printer.cfg) – source configuration
- [`macros.md`](macros.md) – readable reference for the EDTV4 custom macros
- [`../../docs/electronics_pinout.md`](../../docs/electronics_pinout.md) – firmware-derived Octopus Pro connection overview
- [`../../docs/calibration_and_maintenance.md`](../../docs/calibration_and_maintenance.md) – calibration values and maintenance notes

## Configuration authority

For public EDTV4 documentation, the current `printer.cfg` is treated as the authoritative source for installed firmware settings and configured limits. Older manuals or project notes may contain historical values.

## License

The configuration files and macros in this directory are licensed under
GPL-3.0-only.

See [`../../LICENSE.md`](../../LICENSE.md) for details.
