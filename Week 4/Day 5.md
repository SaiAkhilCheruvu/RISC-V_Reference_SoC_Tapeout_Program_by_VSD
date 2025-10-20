# CMOS Power Supply And Device Variation Robustness Evaluation (Day 5)

<img width="1205" height="751" alt="image" src="https://github.com/user-attachments/assets/a7d6ed8f-5f14-4a4c-a0ed-c9f679b3573f" />

<img width="1200" height="794" alt="image" src="https://github.com/user-attachments/assets/ef51ea9e-0bcc-45f5-9f8c-962629e1d549" />

<img width="1212" height="683" alt="image" src="https://github.com/user-attachments/assets/d446c4c9-e6c4-4ccf-875e-7fc1e843f5bb" />


<img width="1209" height="686" alt="image" src="https://github.com/user-attachments/assets/5f48472f-d9c6-4b04-80d2-10572e6a8e86" />


<img width="1211" height="475" alt="image" src="https://github.com/user-attachments/assets/e7861c84-0da0-4463-af9e-e17596194dfb" />


<img width="1204" height="664" alt="image" src="https://github.com/user-attachments/assets/b7b10480-1b25-4bd2-aed4-88023e0cc3c6" />

<img width="1217" height="575" alt="image" src="https://github.com/user-attachments/assets/d00b8d55-ec49-43e4-82a3-c8ca13dbfa0e" />

<img width="1122" height="635" alt="image" src="https://github.com/user-attachments/assets/8a88724a-a84b-492d-baf3-99b7cd6b86ee" />

<img width="1204" height="689" alt="image" src="https://github.com/user-attachments/assets/2e163da7-e4b1-4bb5-a172-cf1bd663b769" />

<img width="1195" height="636" alt="image" src="https://github.com/user-attachments/assets/073b98aa-aaa8-4f47-9a9e-a172e5e85140" />


<img width="1170" height="589" alt="image" src="https://github.com/user-attachments/assets/82a73f1a-30bc-4a55-99c8-ede468032f6f" />






## Labs On Day 5

## Simulation 1 - Inverter Device Variation

### Step 1: Navigate to the Design Directory

We start by moving into the design directory where the Day 5 SPICE files are located:

```
cd sky130CircuitDesignWorkshop/design/
```

This ensures we are in the correct folder for running the SPICE simulations.


### Step 2: Inverter Device Variation Analysis

The simulation file for device variation is:

```
day5_inv_devicevariation_wp7_wn042.spice
```

#### Info about the File Being Simulated

This SPICE file simulates a CMOS inverter while varying transistor sizing to observe its impact on inverter behavior.

**Transistor parameters:**

- *PMOS Width (Wp)* = 7 µm

- *NMOS Width (Wn)* = 0.42 µm

**Purpose:** To study how changes in transistor dimensions affect propagation delay, switching thresholds, and output characteristics.

**Simulation Type:** Transient or DC analysis, depending on the netlist setup.


### Step 3: View the File Contents

We inspect the SPICE netlist using:

```
vim day5_inv_devicevariation_wp7_wn042.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 20-31-16" src="https://github.com/user-attachments/assets/397553d3-9f7b-46a2-ac5c-ab0c16dfa239" />


Open the SPICE file in vim to review transistor sizes, connections, and simulation setup.


### Step 4: Run the Simulation

We run the simulation in NGSPICE:

```
ngspice day5_inv_devicevariation_wp7_wn042.spice
plot out vs in
```

<img width="1139" height="755" alt="Screenshot from 2025-10-15 21-56-04" src="https://github.com/user-attachments/assets/e4aeebbc-7374-4541-9722-9b7929189f47" />

<img width="1139" height="269" alt="Screenshot from 2025-10-15 21-57-39" src="https://github.com/user-attachments/assets/360558dc-9ab5-4769-b395-14f0edc562a8" />


This produces the input and output waveforms of the inverter, allowing us to observe the effects of device size variation.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-56-13" src="https://github.com/user-attachments/assets/b87eedb3-5e48-4df5-a19e-74d3d746b06b" />


## Simulation 2 - Inverter Supply Voltage Variation


### Step 1: Inverter Supply Voltage Variation Analysis

The simulation file for supply variation is:

```
day5_inv_supplyvariation_Wp1_Wn036.spice
```

#### Info about the File Being Simulated

This SPICE file simulates the CMOS inverter under different supply voltage (VDD) conditions.

**Transistor parameters:**

- *PMOS Width (Wp)* = 1 µm

- *NMOS Width (Wn)* = 0.36 µm

**Purpose:** To observe how VDD variations impact inverter performance, including logic levels, propagation delay, and noise margins.

**Simulation Type:** Transient or DC analysis depending on the netlist.


### Step 2: View the File Contents

We open the netlist in vim:

```
vim day5_inv_supplyvariation_Wp1_Wn036.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 20-32-01" src="https://github.com/user-attachments/assets/8d12f38b-1a8c-4178-b1f0-ebdb1d4de5f1" />


Letting us review the SPICE file to see the supply voltage setup and inverter configuration.


### Step 3: Run the Simulation

We run the simulation in NGSPICE:

```
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice
plot out vs in
```

<img width="1139" height="712" alt="Screenshot from 2025-10-15 21-58-40" src="https://github.com/user-attachments/assets/1f16a6c5-890f-4d28-9947-9cd079538d77" />

<img width="1139" height="246" alt="Screenshot from 2025-10-15 21-59-59" src="https://github.com/user-attachments/assets/498689ef-c5c3-4e00-a1fa-857745b5cdf2" />


This generates the inverter waveforms under varying supply voltages, helping us evaluate performance changes.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-59-46" src="https://github.com/user-attachments/assets/65f9ed9a-5f72-43e9-a4a4-77c06a66fd6c" />


## Summary: Day 5 – CMOS Inverter Device and Supply Variation Analysis

**Objective:**

- To study how transistor sizing and supply voltage variations affect CMOS inverter behavior.

- To evaluate the impact on propagation delay, switching thresholds, and output levels.

**Activities Performed:**

**Simulation 1: Device Variation (day5_inv_devicevariation_wp7_wn042.spice):**

- Navigated to the design directory.

- Opened the SPICE file in vim to inspect transistor sizing and circuit setup.

- Ran the simulation in NGSPICE and observed the inverter input/output waveforms.

**Simulation 2: Supply Variation (day5_inv_supplyvariation_Wp1_Wn036.spice):**

- Opened the SPICE file in vim to review supply voltage settings and circuit configuration.

- Executed the simulation in NGSPICE and analyzed the inverter response under different VDD values.

**Key Observations and Learning Points:**

- Observed the impact of transistor size on propagation delay and output levels.

- Evaluated how supply voltage variations affect logic levels, rise/fall times, and noise margins.

- Reinforced understanding of CMOS inverter robustness and design considerations for reliable digital circuits.

**Overall Outcome for Day 5:**

- Learned to analyze inverter performance under device and supply variations.

- Gained practical experience in evaluating real-world design tolerances.

- Completed hands-on characterization of CMOS inverters across multiple operating conditions.

