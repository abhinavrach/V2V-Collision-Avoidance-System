# V2V Collision Avoidance System

Embedded vehicle-to-vehicle communication and collision-warning system on STM32, using time-to-collision computation, an FSM decision layer, and an experimental spiking neural network (SNN) track.

## Overview

Two STM32F446RE Nucleo boards communicate over radio to exchange kinematic data (from an onboard IMU) and compute time-to-collision (TTC) in real time. A finite-state-machine layer converts TTC and sensor input into discrete collision-risk states and driver alerts. A secondary, experimental SNN (leaky integrate-and-fire neurons) explores whether a neuromorphic decision layer can match or improve on the FSM's response.

## Hardware

- 2× STM32F446RE Nucleo boards (one per "vehicle")
- HC-12 radio modules (inter-vehicle link)
- MPU-6050 IMU (motion/orientation sensing)
- Ultrasonic sensors (proximity/ranging)

## Design Decisions

- **Radio module: HC-12, not nRF24L01.** Initial implementation used nRF24L01, but TX failures under test conditions and its short effective range made it unsuitable for realistic V2V distances. Switched to HC-12 (UART-based, longer range) after debugging.

## Pipeline

1. **Sensing** — IMU + ultrasonic data collected on each node
2. **Communication** — kinematic state broadcast between vehicles over HC-12
3. **TTC computation** — time-to-collision calculated from relative position/velocity
4. **Decision layer** — FSM maps TTC + sensor state to discrete alert levels
5. **(Experimental) SNN track** — LIF-neuron based decision layer, evaluated against the FSM baseline

## Results

_Not yet populated — add TTC accuracy, HC-12 effective range/latency under test conditions, and FSM/SNN false-positive and false-negative rates before publishing. A collision system with no quantified detection performance is not a complete result._

## Repo Structure

```
├── firmware/
│   ├── vehicle_node/       # STM32 firmware per node
│   ├── ttc/                # TTC computation
│   ├── fsm/                # FSM decision layer
│   └── snn/                # LIF-neuron experimental decision layer
├── comms/                  # HC-12 UART driver/protocol
├── sensors/                # IMU + ultrasonic drivers
├── test/                   # test logs, range/latency data
└── docs/                   # block diagrams, state charts
```

## Build

- STM32CubeIDE (tested on 2.2.0 — note: known issue with missing `.ioc` file on project creation in this version; regenerate via CubeMX if affected)
- HAL drivers for UART, I2C (IMU), GPIO (ultrasonic)

## Known Limitations

- SNN decision layer is experimental and not yet benchmarked against the FSM baseline on shared test data
- No quantified TTC accuracy or false-positive/false-negative rates yet (see Results)
- HC-12 range/latency not characterized under realistic test conditions

## License

MIT (or specify)
