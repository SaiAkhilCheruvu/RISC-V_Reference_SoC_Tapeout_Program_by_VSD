
# Labs On Day2

## Day 1 – Simulation 1: ID vs VDS at Constant VGS

### Step 1: Navigate to the Desired Directory

Before starting the Day 2 simulations, move into the design directory that contains the circuit files. This ensures you’re in the correct working folder for running the SPICE simulations.

```
cd sky130CircuitDesignWorkshop/design/
```

This directory contains all the .spice files needed for the Day 2 experiments.

### Step 2: Simulate and Plot the (I<sub>D</sub> vs V<sub>DS</sub> at constant V<sub>GS</sub>) Characteristics

The first simulation for Day 2 involves analyzing the ID–VDS characteristics of an NMOS transistor with a smaller geometry compared to Day 1. The simulation file used for this purpose is:

```
day2_nfet_idvds_L015_W039.spice
```

#### Info about the File Being Simulated:

This SPICE file defines the circuit setup for obtaining the drain current (ID) versus drain-to-source voltage (VDS) curves of a scaled NMOS transistor.
It performs a DC sweep of VDS for different constant gate voltages (VGS) to study how scaling down the device dimensions affects its electrical behavior.

**Transistor parameters:**

*Channel Length (L)* = 0.15 µm

*Channel Width (W)* = 0.39 µm

**Purpose:** To compare the ID–VDS characteristics of a smaller device with the larger one from Day 1 and observe changes in saturation current and output resistance.

**Simulation Type:** DC sweep (VDS sweep for multiple VGS levels).


#### View the File's Contents:

Open the SPICE netlist in the vim text editor, allowing you to see transistor definitions, voltage sources, and simulation commands. You can also edit parameters if needed.

```
vim day2_nfet_idvds_L015_W039.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 16-54-03" src="https://github.com/user-attachments/assets/4318ddbc-cc54-4e47-a57d-7dde075005ce" />



#### Run the Simulation:

After reviewing the file, run it in NGSPICE:

```
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```



<img width="1137" height="667" alt="Screenshot from 2025-10-15 21-05-56" src="https://github.com/user-attachments/assets/716e10dd-52cd-47d1-9f5b-6c620abd4de4" />


<img width="1137" height="196" alt="Screenshot from 2025-10-15 21-07-35" src="https://github.com/user-attachments/assets/21b4af15-ce42-4a17-982c-d2555b33215d" />


This will generate the ID vs VDS curves for the specified VGS values.


<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-09-50" src="https://github.com/user-attachments/assets/548cdd19-2309-4747-a319-a5f86927049b" />


## Day 2 – Simulation 2: ID vs VGS at Constant VDS

We now analyze the transfer characteristics of the NMOS transistor by plotting ID vs VGS at a fixed VDS. The simulation file for this task is:

```
day2_nfet_idvgs_L015_W039.spice
```

### Info about the File Being Simulated:

This SPICE file defines the circuit setup for obtaining the drain current (ID) versus gate voltage (VGS) curve of the NMOS transistor.
It allows us to study key transistor parameters such as threshold voltage (Vth) and transconductance (gm).

**Transistor parameters:**

*Channel Length (L)* = 0.15 µm

*Channel Width (W)* = 0.39 µm

**Purpose:** To observe how ID increases with VGS beyond the threshold region and understand the transistor’s switching behavior.

**Simulation Type:** DC sweep of VGS at a constant VDS.

### Step 1: View the File's Contents

We start by inspecting the netlist to understand the transistor parameters and simulation setup:

```
vim day2_nfet_idvgs_L015_W039.spice
```

Open the SPICE file in vim, allowing us to review or modify the transistor definitions, voltage sources, and sweep commands.


<img width="1280" height="800" alt="Screenshot from 2025-10-18 18-12-17" src="https://github.com/user-attachments/assets/a14461b6-295a-4d7a-8393-eaf1a1304a0f" />


### Step 2: Run the Simulation:

Next, we run the file in NGSPICE to generate the waveform:

```
ngspice day2_nfet_idvgs_L015_WN039.spice
plot -vdd#branch
```

<img width="1137" height="667" alt="Screenshot from 2025-10-15 21-07-45" src="https://github.com/user-attachments/assets/36012c98-cdbc-429c-a4ca-0f9cadab37b7" />

<img width="1137" height="272" alt="Screenshot from 2025-10-15 21-08-59" src="https://github.com/user-attachments/assets/e49595c0-3387-4533-a465-879a199430ba" />



This produces the ID vs VGS curve, from which we can estimate the threshold voltage and evaluate the device’s transfer characteristics.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-09-15" src="https://github.com/user-attachments/assets/54c0ce0c-6df6-4a13-a09d-b584507a7123" />


## Summary

**Day 2: Scaled NMOS Characteristics (ID vs VDS and ID vs VGS)**

**Objective:**

- To study how device scaling affects NMOS transistor behavior.

- To analyze both output characteristics (ID vs VDS) and transfer characteristics (ID vs VGS) of a smaller NMOS device.

**Activities Performed:**

*Simulation 1: ID vs VDS*

- Navigated to the design directory.

- Inspected day2_nfet_idvds_L015_W039.spice using vim.

- Ran the simulation in NGSPICE and plotted ID vs VDS for different VGS values.

*Simulation 2: ID vs VGS*

- Opened day2_nfet_idvgs_L015_W039.spice with vim to review the circuit setup.

- Executed the simulation in NGSPICE and plotted ID vs VGS at constant VDS.

**Key Observations and Learning Points:**

- Observed the effect of reduced channel length and width on ID–VDS curves (saturation current decreased slightly, output resistance changed).

- Determined threshold voltage (Vth) and studied the transistor’s switching behavior from the ID–VGS curve.

- Reinforced understanding of DC sweeps, SPICE file structure, and NGSPICE plotting.
