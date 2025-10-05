# Post-Synthesis GLS Of VSDBabySoC


## What is Gate Level Simulation (GLS)

Gate-level simulation models a digital circuit using actual logic gates and their interconnections, including propagation delays, glitches, and timing behavior. Unlike RTL simulation, it shows how the design will behave post-synthesis, reflecting the real hardware more accurately.


| Feature                         | RTL Simulation                                    | Gate-Level Simulation                                     |
| ------------------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| **Abstraction Level**           | High-level, behavioral description of the circuit | Low-level, uses actual logic gates and interconnections   |
| **Purpose**                     | Verify design functionality early                 | Verify post-synthesis design correctness and timing       |
| **Timing Accuracy**             | Ignores gate delays                               | Includes propagation delays, glitches, and timing effects |
| **Simulation Speed**            | Faster                                            | Slower due to detailed gate-level modeling                |
| **Post-Synthesis Verification** | No                                                | Yes                                                       |
| **Glitch/Hazard Detection**     | Limited                                           | Can detect hazards and glitches due to gate delays        |
| **Use Case**                    | Early functional verification, design exploration | Final verification before fabrication                     |




## Why Perform GLS?

GLS is performed to:

- Verify correctness after synthesis.

- Check timing issues like setup/hold violations or glitches.

- Ensure the circuit works reliably under real hardware conditions before fabrication.



## Highlights Of GLS

### 1. Timing Concepts

- **Propagation delay:** Time for a signal to travel through a gate.

- **Setup time:** Minimum time before a clock edge that data must be stable.

- **Hold time:** Minimum time after a clock edge that data must remain stable.

- **Clock skew & timing violations:** Differences in clock arrival can cause setup/hold violations.


### 2. Synthesis and Optimization

- Converts RTL into gates for hardware implementation.

- **Optimizations:** Improve speed, reduce area, or save power.


### 3. Glitch Analysis / Hazard Detection

- **Static hazards:** Unwanted glitches on single transitions.

- **Dynamic hazards:** Multiple unintended transitions due to delays.

- Gate-level simulation is key to detecting hazards missed in RTL simulation.


### 4. Testbench Design

- Includes stimulus generation, assertions, and waveform checking.

- Gate-level simulation often requires more detailed testbenches than RTL.


### 5. Power and Area Estimation

- Gate-level simulation helps estimate real hardware power consumption and circuit area.


### 6. Common Tools and Flow

- **Simulators:** ModelSim, VCS, QuestaSim.

- **Synthesis tools:** Synopsys Design Compiler, Cadence Genus.

- **Typical flow:** RTL design → Synthesis → Gate-level netlist → Gate-level simulation → Timing/power analysis.


## Lab On GLS Implementation For VSDBabySoC

### Open Top-Level Design with Supporting Modules

```
yosys
```
