# Haptics system (Simulink) for the high-realism simulator at IRGLM

This Simulink system reproduces the behaviour of a manual wheelchair
propelled either in straight or curvilinear direction, and controls
motorized rollers to match the velocity of the virtual wheelchair.

Linear velocity (i.e., the mean of both rollers velocity) is controlled
using an admittance loop which takes the anteroposterior force as an
input.

Angular velocity (i.e., the difference between both rollers velocities)
is controlled by applying a proportional compensator on the difference
of rolling resistance.

## From Simulink to Godot

This one-way communication uses this UDP protocol:

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | N/A (ignored)                                 |
| double            | N/A (ignored)                                 |

where:
- `index` is a 4-bit counter that increments at each data packet
- `state` is:

| Bit | Description                  |
| --- | ---------------------------- |
| 0   | Stopped (from digital input) |

- and `command` is:

### CMD = 0: No operation

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | N/A (ignored)                                 |
| double            | N/A (ignored)                                 |

### CMD = 1: Speed update

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | linear speed (m/s)                            |
| double            | angular speed (rad/s)                         |



## From Godot to Simulink

This one-way communication uses this UDP protocol:

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | N/A (ignored)                                 |
| double            | N/A (ignored)                                 |

where:
- `index` is a 4-bit counter that increments at each data packet
- `state` is:

| Bit | Description                                                 |
| --- | ----------------------------------------------------------- |
| 0   | Enable (Then sent to the Hardware Enable chain by Simulink) |

- and `cmd` is:

### CMD = 0: No operation

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | N/A (ignored)                                 |
| double            | N/A (ignored)                                 |
|                   |                                               |

### CMD = 1: Inertia update

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | mass (kg)                                     |
| double            | moment of inertia (kg.m^2) (ignored for now)  |

### CMD = 2: Rolling resistance update

| Type              | Value                                         |
| ----------------- | --------------------------------------------- |
| long unsigned int | index (4bits) - cmd (12bits) - state (16bits) |
| double            | forward rolling resistance coefficient        |
| double            | backward rolling resistance coefficient       |
