# CMOS Noise Margin Robustness Evaluation (Day 4)

<img width="474" height="246" alt="image" src="https://github.com/user-attachments/assets/717d7dea-4ea5-4228-9792-a68338b6010e" />

<img width="1210" height="734" alt="image" src="https://github.com/user-attachments/assets/1800364d-6349-4f09-a6f0-ab43996ebf7f" />

<img width="1205" height="750" alt="image" src="https://github.com/user-attachments/assets/73df7289-8066-40af-8602-f6e4da40a69b" />

<img width="1215" height="681" alt="image" src="https://github.com/user-attachments/assets/ccfe02ae-65ee-4ee0-b03d-2f26cebfe9d4" />

<img width="1215" height="688" alt="image" src="https://github.com/user-attachments/assets/be95fa72-832c-46cd-b524-1777a75df982" />

<img width="1204" height="675" alt="image" src="https://github.com/user-attachments/assets/d03e0274-1f08-452a-95e0-7a5edc871571" />





















## Labs On Day 4

## Step 1: Navigate to the Design Directory

We start by moving into the design directory where the Day 4 SPICE file is located:

```
cd sky130CircuitDesignWorkshop/design/
```

This ensures we are in the correct folder for running the SPICE simulation.


## Step 2: Analyze CMOS Inverter Noise Margin

The simulation file for Day 4 is:

```
day4_inv_noisemargin_wp1_wn036.spice
```

## Info about the File Being Simulated

This SPICE file sets up a CMOS inverter and performs a DC sweep to evaluate its noise margins.

### Transistor parameters:

*PMOS Width (Wp)* = 1 µm

*NMOS Width (Wn)* = 0.36 µm

**Purpose:** To determine the inverter’s logic high (VOH), logic low (VOL), and noise margin levels (NMH, NML).

**Simulation Type:** DC sweep of Vin.


## Step 3: View the File's Contents

We inspect the netlist using:

```
vim day4_inv_noisemargin_wp1_wn036.spice
```

<img width="1280" height="800" alt="Screenshot from 2025-10-18 20-18-04" src="https://github.com/user-attachments/assets/069e6be9-ee9d-46dd-b191-0d02385e813b" />


Open the SPICE file in vim, allowing us to review transistor sizing, input sweep range, and output node definitions.


## Step 2b: Run the Simulation

We run the simulation in NGSPICE:

```
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```

<img width="1139" height="710" alt="Screenshot from 2025-10-15 21-38-00" src="https://github.com/user-attachments/assets/29dc3d93-9f98-403f-a2b3-238120282889" />

<img width="1139" height="269" alt="Screenshot from 2025-10-15 21-38-13" src="https://github.com/user-attachments/assets/996f654a-5b41-4ded-b1ab-9a126820bca9" />


This generates the Vout vs Vin curve, from which we can calculate the inverter’s noise margins (NMH and NML).

<img width="1280" height="800" alt="Screenshot from 2025-10-15 21-38-18" src="https://github.com/user-attachments/assets/19220ad1-b9a2-479b-a7c7-9600c470cc03" />


## Summary: Day 4 – CMOS Inverter Noise Margin Analysis

**Objective:**

- To evaluate the noise immunity of a CMOS inverter.

- To determine logic levels and noise margin values for robust digital circuit design.

**Activities Performed:**

- Navigated to the design directory.

- Opened the SPICE file in vim to inspect transistor sizes and DC sweep setup.

- Executed the simulation in NGSPICE and plotted Vout vs Vin.

- Measured VOH, VOL, NMH, and NML from the VTC curve.

**Key Observations and Learning Points:**

- Identified the logic high (VOH) and logic low (VOL) levels.

- Calculated noise margins (NMH, NML) to evaluate inverter tolerance to input noise.

- Understood the impact of transistor sizing on inverter noise margins.

- Reinforced skills in DC sweep analysis and inverter characterization.

**Overall Outcome for Day 4:**

- Learned how to quantify inverter noise margins using SPICE simulations.

- Gained practical understanding of how CMOS inverters maintain reliable logic operation despite input noise.

- Prepared for more advanced CMOS circuit analyses involving device and supply variations.
