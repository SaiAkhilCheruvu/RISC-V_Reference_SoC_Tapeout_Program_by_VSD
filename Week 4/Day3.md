# Labs On Day 3


## Simulation 1 - Transient analysis


### Step 1: Navigate to the Design Directory

We start by moving into the design directory where the Day 3 circuit files are located:

```
cd sky130CircuitDesignWorkshop/design/
```

This ensures we are in the correct folder for running the SPICE simulations.


### Step 2: Transient Analysis of a CMOS Inverter

We analyze the time-domain response of a CMOS inverter using the following SPICE file:

```
day3_inv_tran_Wp084_Wn036.spice
```

### Info about the File Being Simulated

This SPICE file defines a CMOS inverter and performs a transient analysis to simulate its switching response when the input voltage changes over time.

**Transistor parameters:**

- *PMOS Width (Wp)* = 0.84 µm

- *NMOS Width (Wn)* = 0.36 µm

**Purpose:** To study the inverter’s rise time, fall time, and propagation delay.

**Simulation Type:** Transient analysis (time-domain).


### Step 3: View the File Contents

We inspect the SPICE netlist using:

```
vim day3_inv_tran_Wp084_Wn036.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 20-04-34" src="https://github.com/user-attachments/assets/ed182767-4a54-4039-8cb6-6d80f9f32a35" />


Open the netlist in vim so we can review transistor connections, input waveform, and transient analysis commands.


### Step 4: Run the Transient Simulation

We execute the simulation in NGSPICE:

```
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```

<img width="1137" height="689" alt="Screenshot from 2025-10-15 21-23-32" src="https://github.com/user-attachments/assets/974114f3-6b3f-497f-9aa2-318d8a05191d" />

<img width="1137" height="225" alt="Screenshot from 2025-10-15 21-29-51" src="https://github.com/user-attachments/assets/0826d46d-4e8d-4813-960e-b96c266a710a" />


This generates the input and output waveforms of the CMOS inverter, allowing us to measure rise/fall times and propagation delay.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-29-58" src="https://github.com/user-attachments/assets/885b96c8-f3eb-4a95-b84f-21c15d7d6a59" />



## Simulation 2 - Voltage Transfer Chracteristics (VTC)

### Step 1: Voltage Transfer Characteristics (VTC) of a CMOS Inverter

Next, we analyze the static behavior of the same inverter using:

```
day3_inv_vtc_Wp084_Wn036.spice
```

### Info about the File Being Simulated

This SPICE file performs a DC sweep of the inverter’s input voltage to generate the Voltage Transfer Characteristics (VTC).

**Transistor parameters:**

- *PMOS Width (Wp)* = 0.84 µm

- *NMOS Width (Wn)* = 0.36 µm

**Purpose:** To determine logic thresholds, switching point, and noise margins of the inverter.

**Simulation Type:** DC sweep of Vin.


### Step 2: View the File Contents

We check the netlist with:

```
vim day3_inv_vtc_Wp084_Wn036.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 20-05-11" src="https://github.com/user-attachments/assets/f831c5ce-3927-480a-91d3-61c2b7db2de5" />


Allowing us to see the DC sweep setup, transistor sizing, and output node definition.


### Step 3: Run the VTC Simulation

We run the simulation in NGSPICE:

```
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs time in
```

<img width="1137" height="710" alt="Screenshot from 2025-10-15 21-30-50" src="https://github.com/user-attachments/assets/d58e51ef-bc3c-420d-ac5d-8f00ce99ee97" />

<img width="1137" height="381" alt="Screenshot from 2025-10-15 21-32-52" src="https://github.com/user-attachments/assets/fad418c5-17ab-4a95-b4cb-a0dbca4e3e4c" />


This produces the Vout vs Vin curve, from which we can determine the inverter’s switching threshold and noise margins.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-33-01" src="https://github.com/user-attachments/assets/7628f29c-6316-4261-9c74-47e0ccf54da5" />


## Summary: Day 3 – CMOS Inverter Analysis

### Objective:

- To study both the dynamic and static behavior of a CMOS inverter using Sky130 NMOS and PMOS devices.

- To measure transient response parameters such as rise time, fall time, and propagation delay, and to understand inverter switching thresholds and noise margins.

### Activities Performed:

**Simulation 1: Transient Analysis (day3_inv_tran_Wp084_Wn036.spice)**

- Navigated to the design directory.

- Opened the SPICE file in vim to inspect transistor sizing, input waveform, and transient analysis commands.

- Ran the simulation in NGSPICE and plotted input and output voltages.

**Simulation 2: Voltage Transfer Characteristics (VTC) (day3_inv_vtc_Wp084_Wn036.spice)**

- Opened the SPICE file in vim to review the DC sweep setup and inverter connections.

- Executed the simulation in NGSPICE to generate the Vout vs Vin curve.

### Key Observations and Learning Points:

- Observed the propagation delay, rise time, and fall time of the inverter from transient analysis.

- Determined the switching threshold voltage and noise margins from the VTC curve.

- Reinforced understanding of CMOS inverter operation, including how transistor sizing affects speed and logic levels.

- Gained experience in combining transient and DC analyses to fully characterize a digital gate.

### Overall Outcome for Day 3:

- Developed hands-on skills in simulating CMOS inverter behavior using Sky130 PDK.

- Learned to analyze both dynamic and static characteristics of a digital logic gate.

- Built a foundation for more complex CMOS circuits in future labs.


