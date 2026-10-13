# EDTV4 System Architecture

The diagram below gives a high-level view of the EDTV4 control and machine architecture.

```mermaid
flowchart TB
    User[Operator / OrcaSlicer] --> Host[Raspberry Pi 4\nKlipper host + Mainsail]
    Host -->|USB| MCU[BIGTREETECH Octopus Pro v1.1\nSTM32H723]

    MCU --> X0[E0 / Left X carriage]
    MCU --> X1[E1 / Right X carriage]
    MCU --> Y[Y axis\n2 motors]
    MCU --> Z[Z axis\n2 motors]

    MCU --> E0[E0 extruder\nTMC2209 + hotend]
    MCU --> E1[E1 extruder\nTMC2209 + hotend]

    MCU --> Bed[Heated bed]
    MCU --> Chamber[Chamber heater]
    MCU --> Fans[Hotend / part cooling / chamber / recirculation fans]

    Probe[Z probe] --> MCU
    Filament[Filament switch + motion sensor] --> MCU
    EStop[Hardware emergency stop] --> MCU
```

## Control flow

1. OrcaSlicer produces machine-specific G-code using one of the tested EDTV4 profile bundles.
2. The G-code is transferred to the Klipper host.
3. Klipper coordinates both X carriages, the shared Y/Z motion system, heaters, fans and sensors through the Octopus Pro controller.
4. Tool changes select the appropriate carriage and extruder, park the inactive carriage and restore the configured tool offset.

## Motion architecture

```mermaid
flowchart LR
    Y1[Y motor 1] --> Gantry[Shared XY gantry]
    Y2[Y motor 2] --> Gantry
    Gantry --> X0[Left carriage E0]
    Gantry --> X1[Right carriage E1]
    Z1[Z motor 1] --> Bed[Build platform]
    Z2[Z motor 2] --> Bed
```

The two X carriages share the Y motion system but move independently along X. This enables independent dual extrusion while also creating different reachable X regions for E0 and E1.

## Thermal architecture

```mermaid
flowchart LR
    MCU[Octopus Pro] --> HE0[E0 hotend]
    MCU --> HE1[E1 hotend]
    MCU --> HE2[Chamber heater]
    MCU --> HE3[Heated bed]
    ChamberSensor[Chamber temperature sensor] --> MCU
    BedSensor[Bed thermistor] --> MCU
    E0Sensor[E0 thermistor] --> MCU
    E1Sensor[E1 thermistor] --> MCU
```

For the exact output and sensor-pin assignments, see [`electronics_pinout.md`](electronics_pinout.md).
