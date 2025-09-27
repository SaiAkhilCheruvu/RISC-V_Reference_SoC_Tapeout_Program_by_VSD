# Day 2

Welcome to Day 2!
In this session, we explored timing libraries, compared hierarchical and flat synthesis methods, and practiced efficient coding styles for flip-flops. These topics are key for optimizing digital designs and preparing for advanced RTL and synthesis workflows.

## Introduction To Timing Libraries

A timing library, often in ```.lib``` format, provides detailed information about standard cell delays, power, and area, enabling synthesis and timing analysis tools to accurately model and optimize digital circuits for real silicon. These libraries characterize how each logic cell behaves under different conditions, allowing designs to meet performance and timing requirements reliably.

### Sky130 PDK?

The Sky130 PDK (Process Design Kit) is an open-source library of design rules, device models, and standard cells for the SkyWater 130nm semiconductor fabrication process. It allows designers, researchers, and students to create, simulate, and fabricate chips using a commercially viable 130nm technology node without requiring NDAs or costly licenses.

To open and view the tech library file ```sky130_fd_sc_hd__tt_025C_1v80.lib``` using gvim from the workshop directory in terminal, run:

```
gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1280" height="800" alt="Screenshot from 2025-09-27 08-09-49" src="https://github.com/user-attachments/assets/920ab074-eca7-4274-8db0-b881a8fd81c5" />


This command opens the liberty (.lib) file in the text editor gvim, allowing you to inspect timing, power, and structural characterization data of standard cells used in the Sky130 process design kit for synthesis and analysis.

The file should be opened for *viewing only*, as it contains precise timing, power, and structural data for synthesis and analysis. It can be broken down as follows:

#### The Library Components:

- **sky130:** the 130nm SkyWater process node

- **fd:** fully depleted transistor technology

- **sc:** standard cells library containing basic digital logic gates and flip-flops

- **hd:** high density, optimized for compact chip area

#### The PVT (Process, Voltage, Temperature) components:

- **tt:** typical process corner representing nominal transistor performance

- **025C:** temperature of 25 degrees Celsius

- **1v80:** supply voltage of 1.8 volts

The Sky130 FD-SC-HD standard cell library (sky130_fd_sc_hd) contains a large variety of digital cells. Each cell features detailed characterization data, including leakage power, dynamic power, timing, and area values. These parameters help synthesis and analysis tools optimize designs for power efficiency, speed, and silicon area usage.

To view the features of the ``` and2``` standard cell (2-input AND Gate) , we must run the following command:

```
:sp ../my_lib/verilog_model/sky130_fd_sc_hd__and2.behavioral.v
```

## Synthesis Approaches: Hierarchical vs Flat

Hierarchical and flat synthesis are two approaches to logic synthesis in digital design.

### Hierarchical Synthesis

**What it is:** Keeps the original module structure from your RTL code, synthesizing each module separately.

**How it works:** Synthesis tools process each module on its own, maintaining clear boundaries between blocks.

**Benefits:**

- Faster synthesis for large or complex designs

- Easier debugging and analysis, since module boundaries are preserved

- Modular approach helps with design reuse and tool integration

**Limitations:**

- Limited optimization across module boundaries

- Some reports or timing checks may need extra setup

  <img width="1280" height="800" alt="Screenshot from 2025-09-21 22-19-16" src="https://github.com/user-attachments/assets/162e1863-3ac1-44c6-8938-3d390e503c54" />


### Flat Synthesis

**What it is:** Flattens the entire design into a single-level netlist before synthesis, removing module boundaries.

**How it works:** The synthesis tool combines all modules into one big block, then optimizes the whole design together.

**Benefits:**

- Allows for cross-module optimizations, which can improve performance and reduce area

- Can lead to more efficient final hardware for smaller or less complex designs

**Limitations:**

- Synthesis can be slower and require more memory for large designs

- Debugging is harder, since the original module structure is lost

- Less modular, making design reuse and integration more challenging

  <img width="1280" height="800" alt="Screenshot from 2025-09-21 22-37-16" src="https://github.com/user-attachments/assets/b77f0740-d5bd-4a9f-8328-d615e14e316c" />

  

| Feature            | Hierarchical Synthesis                | Flat Synthesis                        |
|--------------------|---------------------------------------|---------------------------------------|
| Module Handling    | Keeps modules separate                | Merges all modules into one           |
| Optimization Scope | Within each module                    | Across the entire design              |
| Synthesis Speed    | Faster for big projects               | Slower as design size increases       |
| Debugging Ease     | Easier (module boundaries visible)    | Harder (structure is flattened)       |
| Output Structure   | Modular netlist                       | Single, complex netlist               |
| Best Use Case      | Modular, incremental development      | Maximum optimization/performance      |


