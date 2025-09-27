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

-> *sky130:* the 130nm SkyWater process node

-> *fd:* fully depleted transistor technology

-> *sc:* standard cells library containing basic digital logic gates and flip-flops

-> *hd:* high density, optimized for compact chip area

#### The PVT (Process, Voltage, Temperature) components:

-> *tt:* typical process corner representing nominal transistor performance

-> *025C:* temperature of 25 degrees Celsius

-> *1v80:* supply voltage of 1.8 volts

The Sky130 FD-SC-HD standard cell library (sky130_fd_sc_hd) contains a large variety of digital cells. Each cell features detailed characterization data, including leakage power, dynamic power, timing, and area values. These parameters help synthesis and analysis tools optimize designs for power efficiency, speed, and silicon area usage.

To view the features of the ``` and2``` standard cell (2-input AND Gate) , we must run the following command:

```
:sp ../my_lib/verilog_model/sky130_fd_sc_hd__and2.behavioral.v
```

