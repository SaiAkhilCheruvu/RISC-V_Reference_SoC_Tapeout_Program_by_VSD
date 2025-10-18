# INTRODUCTION TO CIRCUIT DESIGN AND SPICE SIMUATIONS (Day 1)


## What is Circuit Design?


Circuit design is the process of creating electrical circuits that perform specific functions. It involves selecting and connecting components like resistors, capacitors, diodes, and ICs to achieve a desired output. Designs are first planned using schematics and simulations, and then implemented on breadboards or PCBs for testing and real-world use.


## What is SPICE?

SPICE (Simulation Program with Integrated Circuit Emphasis) is a circuit simulation tool used to test and analyze electronic circuits on a computer before building them. It models components like resistors and transistors using math equations to predict how the circuit will behave under different conditions. In simple terms, SPICE acts like a virtual electronics lab that helps you debug and optimize your design early in the process.


## Why do we need SPICE Simulations?

SPICE (Simulation Program with Integrated Circuit Emphasis) is essential for analyzing electronic circuits before building them. Key reasons include:

**1. Circuit Verification**

Test designs virtually to catch flaws and avoid costly hardware mistakes.

**2. Performance Analysis**

Study voltage, current, power, and timing under various conditions.

**3. Design Optimization**

Fine-tune component values and explore multiple scenarios quickly.

**4. Handling Complex Designs**

Analyze large or mixed-signal circuits that are hard to solve manually.

**5. Cost and Time Savings**

Reduce prototyping costs and speed up development cycles.


<img width="913" height="768" alt="1 new message - Brave 16-10-2025 20_06_01" src="https://github.com/user-attachments/assets/74917551-ef19-4ccb-9737-b2b6175c7915" />

## LABS ON DAY-1

### Step 1: Clone the Repository

To begin working with SPICE simulations using the Sky130 process, you’ll first need to obtain the necessary model files and example circuits. These resources are available in a public GitHub repository. Clone the repository to your local machine using the command below:

```
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```

This will download all the Sky130 model files and circuit examples required for simulation and analysis.

<img width="1280" height="800" alt="Screenshot from 2025-10-18 16-21-16" src="https://github.com/user-attachments/assets/4ab7a3ed-dc32-48e0-a04f-e407f08be3f9" />

### Step 2: Navigate to the Desired Directory:

Once the repository has been successfully cloned, move into the directory that contains the design files for the SPICE simulations. This folder includes various example circuits and setup files needed for running simulations. Use the following command to navigate to it:

```
cd sky130CircuitDesignWorkshop/design/
```

**View The File's Contents:**

Open the SPICE netlist in the vim text editor, allowing you to see transistor definitions, voltage sources, and simulation commands. You can also edit parameters if needed.

<img width="1280" height="800" alt="Screenshot from 2025-10-15 19-53-53" src="https://github.com/user-attachments/assets/0116535a-154c-446c-a89a-f381c3377366" />



This places you in the main working directory where you can access and modify circuit files as needed for your Sky130-based designs.


### Step 3: Plot the (I<sub>D</sub> vs V<sub>DS</sub> at constant V<sub>GS</sub>) Characteristics Using NGSPICE:

After navigating to the design directory, you can now simulate and visualize the transistor characteristics. In this step, you’ll generate the I<sub>D</sub> vs V<sub>DS</sub> at constant V<sub>GS</sub> using NGSPICE. The simulation file provided for this purpose is:
 
```
day1_nfet_idvds_L2_W5.spice
```

**Info about the File Being Simulated:**

This SPICE file contains the circuit netlist for plotting the ID–VDS characteristics of an NMOS transistor using the Sky130 process.
It defines transistor parameters and bias conditions required to observe how drain current (ID) varies with drain-to-source voltage (VDS) for different gate voltages (VGS).

**Transistor parameters:**

Channel Length (L) = 2 µm

Channel Width (W) = 5 µm

**Purpose:** To study the transistor’s output characteristics and identify its linear and saturation regions.

**Simulation Type:** DC sweep of VDS for multiple VGS values.

To run the simulation and view the waveform, execute the following commands:

```
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```

<img width="1280" height="800" alt="Screenshot from 2025-10-15 19-56-08" src="https://github.com/user-attachments/assets/13bb1ed9-0c69-4429-96fb-1098cb5fb066" />


<img width="1074" height="654" alt="Screenshot from 2025-10-15 19-58-47" src="https://github.com/user-attachments/assets/9c6c1ee1-11b2-47b6-9f35-92499859f2a8" />



Once NGSPICE opens, it will simulate the circuit and allow you to plot the drain current (I<sub>D</sub>) against the Drain-to-Source voltage (V<sub>DS</sub>) for the specified Gate-to-Source Voltage (V<sub>GS</sub>) values, helping you analyze the transistor’s output characteristics.


<img width="1280" height="800" alt="Screenshot from 2025-10-15 19-59-16" src="https://github.com/user-attachments/assets/33bf8dfa-d788-4d93-a357-a6a6648b9f4d" />



