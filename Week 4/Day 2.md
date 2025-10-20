# Velocity Saturation And Basics Of CMOS Inverter VTC (Day 2)

**Why CMOS is Called “Complementary Metal–Oxide–Semiconductor”**

The name CMOS describes both the structure and operation of the technology:

- **Complementary** – It uses both NMOS and PMOS transistors in pairs that work in opposite (complementary) ways. When one conducts, the other is off, greatly reducing power consumption.

- **Metal** – The gate electrode was originally made of metal (now often polysilicon or metal again in modern processes).

- **Oxide** – A thin silicon dioxide (SiO₂) layer insulates the gate from the underlying semiconductor, allowing voltage control without direct current flow.

- **Semiconductor** – The base material is silicon, a semiconductor that forms the transistor body through doping.


![WhatsApp Image 2025-10-20 at 10 15 22_de2bba27](https://github.com/user-attachments/assets/ca075a3c-2da9-4cb5-af04-130a91c6fd2a)


![WhatsApp Image 2025-10-20 at 10 15 22_599b91c5](https://github.com/user-attachments/assets/af372681-33a5-4209-a9ba-31eec12d202d)


![WhatsApp Image 2025-10-20 at 10 15 22_c84b48d7](https://github.com/user-attachments/assets/aaae496d-21cf-4e85-a867-3b1ce120e4d3)


![WhatsApp Image 2025-10-20 at 10 15 22_1ab1ca6a](https://github.com/user-attachments/assets/64b2bc8b-9039-4ef0-93d8-061434a80344)


![WhatsApp Image 2025-10-20 at 10 15 23_404270a3](https://github.com/user-attachments/assets/88827c27-7e9f-450d-8785-cf36af1a7581)


![WhatsApp Image 2025-10-20 at 10 15 23_49ad52d8](https://github.com/user-attachments/assets/14f295ce-9f81-4134-9904-29626760a0e5)


![WhatsApp Image 2025-10-20 at 10 15 23_1c496901](https://github.com/user-attachments/assets/b0af3d81-1a3e-449c-a98d-6ff3ff002f31)


![WhatsApp Image 2025-10-20 at 10 15 24_a5b324bc](https://github.com/user-attachments/assets/1e3449ba-8b22-488d-bcb1-e5df3c4ece2b)


![WhatsApp Image 2025-10-20 at 10 15 24_ebe5d16b](https://github.com/user-attachments/assets/f5051933-d427-494c-a19c-1c03840235b0)


![WhatsApp Image 2025-10-20 at 10 15 25_094f4f7c](https://github.com/user-attachments/assets/ef5665a9-6448-42b3-9a56-008c6772bad2)


![WhatsApp Image 2025-10-20 at 10 15 25_9fd99e2f](https://github.com/user-attachments/assets/d4448e50-d6f7-413b-af51-a3b91021493b)


![WhatsApp Image 2025-10-20 at 10 15 25_7672ee65](https://github.com/user-attachments/assets/2894a2cb-2c23-47e0-88c8-65a415b31191)



![WhatsApp Image 2025-10-20 at 10 15 25_88a6d021](https://github.com/user-attachments/assets/a8b9f655-48f8-453a-b4af-5b7385e40d6d)


![WhatsApp Image 2025-10-20 at 10 16 29_271118f9](https://github.com/user-attachments/assets/73cce58a-5746-43f6-a8ba-d5fe29446639)


![WhatsApp Image 2025-10-20 at 10 16 33_f819094e](https://github.com/user-attachments/assets/5e172e93-837f-4696-bf39-0e61b85378d5)


![WhatsApp Image 2025-10-20 at 10 16 43_f0f276b3](https://github.com/user-attachments/assets/25e061ad-3684-436f-90dd-f018bdf81704)


![WhatsApp Image 2025-10-20 at 10 16 49_810bb5e6](https://github.com/user-attachments/assets/ecc05840-e5b9-4ca2-a8c5-4849eb3c5499)


## LABS ON DAY 2

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
