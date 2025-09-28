# Day 4

Day 4 focused on understanding the importance of gate-level simulation (GLS), the differences between blocking and non-blocking statements in Verilog, and how mistakes in RTL coding styles can result in simulation-synthesis mismatches.


## What Is GLS?

Gate Level Simulation (GLS) is running a testbench on the gate-level netlist (post-synthesis representation). The netlist is a logical equivalent of the RTL, so the same testbench can be used to verify the design at this stage.

## Why Perform GLS?

GLS ensures the timing correctness of the synthesized design, verifying that logical functionality remains intact after synthesis optimizations, and flags timing-related issues missed by RTL simulation.


## When Is GLS Performed?

It is performed after synthesis, before place-and-route, to validate post-synthesis design behavior and timing.


## Types Of GLS?

- Functional GLS: Simulates gate-level netlist without timing delays to verify functional equivalence with RTL.

- Timing-aware GLS: Includes timing delays annotated from Standard Delay Format (SDF) files to check actual timing and detect setup/hold violations and glitches.


  

