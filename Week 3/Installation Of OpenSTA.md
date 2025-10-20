# Installation Of OpenSTA

## 🧮 OpenSTA – Open Source Static Timing Analysis Tool

**OpenSTA** (Open Static Timing Analyzer) is an open-source software tool used for **static timing analysis (STA)** of digital circuits. It evaluates the timing performance of a synthesized or placed-and-routed design **without requiring dynamic simulation**.

---

## 📘 Overview

OpenSTA reads standard design files such as:
- **Verilog (.v)** – gate-level netlists  
- **Liberty (.lib)** – standard cell timing libraries  
- **SDC (.sdc)** – timing constraints  
- **SPEF (.spef)** – parasitic information  

Using these inputs, OpenSTA performs a comprehensive analysis of:
- ✅ **Setup and hold timing checks**  
- ⚡ **Path delays and slack calculations**  
- 🕒 **Clock tree propagation and skew**  
- 🔍 **Critical path identification**

---

## 🧠 What It’s Used For

OpenSTA is a key tool in the **VLSI physical design flow**, commonly used for:
- **Post-synthesis verification**  
- **Place-and-route timing closure**  
- **Sign-off timing validation**

It’s widely adopted in open-source and academic EDA toolchains as well as professional ASIC/SoC design environments.

---

## ⚙️ Key Features

- 🧩 Supports industry-standard input formats (Verilog, Liberty, SDC, SPEF)  
- 🚀 Fast and accurate static timing computation  
- 🧠 Tcl-based scripting and batch-mode operation  
- 🔄 Easy integration with other design tools  
- 🪶 Lightweight and fully open-source (BSD license)

---

## 💻 Example Usage

```bash
### Launch OpenSTA
sta

### Load design files
read_liberty my_lib.lib
read_verilog my_design.v
link_design top_module

### Apply timing constraints
read_sdc constraints.sdc

### Run timing analysis
report_checks
report_tns
report_wns
```

## Installation Of OpenSTA

Follow the steps below to install OpenSTA:

### Step 1 : Clone The Repository

First, download the OpenSTA source code from its official GitHub repository and navigate into the project directory.

```
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
```

### Step 2 : Build The Docker Image

Next, create a Docker image for OpenSTA using the provided Dockerfile. This will set up the necessary environment and dependencies automatically.

```
docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```

<img width="1219" height="717" alt="Screenshot from 2025-10-18 21-52-09" src="https://github.com/user-attachments/assets/ab66aa45-10ab-425b-aae4-2d5d298ec846" />


### Step 3 : Run The OpenSTA Container

Now, start a Docker container using the OpenSTA image. The -v option mounts directories from your system so you can access your data, and -i runs the container interactively.

```
docker run -i -v $HOME:/data opensta
```

Once the container starts successfully, you’ll see the % prompt—this indicates that the OpenSTA interactive shell is ready for use.


### Step 4 : Performing Timing Analysis with Inline Commands

After entering the OpenSTA shell (indicated by the % prompt), you can run basic static timing analysis using the following inline commands:

```
# Load the Liberty timing library file
read_liberty /OpenSTA/examples/nangate45_slow.lib.gz

# Load the gate-level Verilog netlist
read_verilog /OpenSTA/examples/example1.v

# Link the top-level Verilog design with the loaded Liberty cells
link_design top

# Create a 10 ns clock named 'clk' for the inputs clk1, clk2, and clk3
create_clock -name clk -period 10 {clk1 clk2 clk3}

# Define a 0 ns input delay for inputs in1 and in2 relative to clock 'clk'
set_input_delay -clock clk 0 {in1 in2}

# Generate a timing report for the design
report_checks
```

<img width="1219" height="717" alt="Screenshot from 2025-10-18 21-53-37" src="https://github.com/user-attachments/assets/5c5d9d75-6118-4c16-8f06-33fe0d21efa5" />

<img width="1219" height="717" alt="Screenshot from 2025-10-18 21-53-54" src="https://github.com/user-attachments/assets/ec1b462f-7f6c-4d5d-b056-e75bdf07cb9f" />


#### Understanding Setup (Max Delay) Analysis

The current setup represents a setup timing (maximum delay) corner, so OpenSTA focuses on setup checks by default.

Why Does ```report_checks``` Show Only Max (Setup) Paths?

By default, the ```report_checks``` command performs setup timing analysis, equivalent to running:

```
report_checks -path_delay max
```

This means it reports only the maximum path delays, which correspond to setup checks.

To view only hold checks (minimum path delays), run:

```
report_checks -path_delay min
```

If you want to include both setup and hold timing checks, use:

```
report_checks -path_delay min_max
```


<img width="1219" height="679" alt="Screenshot from 2025-10-18 21-54-39" src="https://github.com/user-attachments/assets/8082b658-93f7-4532-bfb4-2942eaf254b8" />


<img width="1219" height="701" alt="Screenshot from 2025-10-18 21-54-48" src="https://github.com/user-attachments/assets/032b5937-53e9-494a-8738-2d3797312314" />


Navigate to:

```
/OpenSTA/examples
```




### Step 5 : Analysing Report Outcomes


The analysis is performed on the Verilog netlist file ```example1.v``` :



<img width="865" height="335" alt="Screenshot from 2025-10-20 13-24-17" src="https://github.com/user-attachments/assets/070bfa4f-b2f6-4030-9012-617b518ff396" />


Use the following commands to run Yosys synthesis on ```example1.v``` :

```
sai-akhil-cheruvu@sai-akhil-cheruvu-VirtualBox:~$ cd ~/OpenSTA/examples/
sai-akhil-cheruvu@sai-akhil-cheruvu-VirtualBox:~/OpenSTA/examples$ yosys

# Load the standard cell library (Liberty file)
yosys> read_liberty -lib nangate45_slow.lib

# Read the Verilog design file
yosys> read_verilog example1.v

# Synthesize the design with 'top' as the top-level module
yosys> synth -top top

# Visualize the synthesized netlist as a schematic
yosys> show
```

<img width="1219" height="677" alt="Screenshot from 2025-10-18 22-05-08" src="https://github.com/user-attachments/assets/a65508e2-e6b1-4750-ace3-df9f504f8b6a" />


<img width="1219" height="715" alt="Screenshot from 2025-10-18 22-05-56" src="https://github.com/user-attachments/assets/0bf8ae6d-391f-4c5a-bbf0-6181b6751975" />


The following netlist diagram was generated using Yosys:

<img width="1152" height="701" alt="Screenshot from 2025-10-20 14-37-10" src="https://github.com/user-attachments/assets/d85d9c73-f6db-4fc4-918b-9e903a68eb3b" />


#### SPEF-Based Timing Analysis

The OpenSTA timing analysis flow can also incorporate SPEF-based parasitic data.

By including post-layout RC parasitics, this approach provides more accurate delay and slack calculations, resulting in improved timing signoff precision. Run the following commands:

```
# Start an interactive OpenSTA Docker container and mount your home directory
docker run -i -v $HOME:/data opensta

# Load the Liberty timing library
read_liberty /OpenSTA/examples/nangate45_slow.lib.gz

# Load the gate-level Verilog netlist
read_verilog /OpenSTA/examples/example1.v

# Link the top-level design
link_design top

# Read SPEF file for parasitic (RC) data
read_spef /OpenSTA/examples/example1.dspef

# Create a 10 ns clock for clk1, clk2, and clk3
create_clock -name clk -period 10 {clk1 clk2 clk3}

# Set 0 ns input delay for in1 and in2 relative to the clock
set_input_delay -clock clk 0 {in1 in2}

# Generate timing report
report_checks
```

<img width="1219" height="580" alt="Screenshot from 2025-10-18 22-08-25" src="https://github.com/user-attachments/assets/9fb9f48e-8997-48dc-bea3-5cc8d726ee73" />

<img width="1219" height="705" alt="Screenshot from 2025-10-18 22-08-34" src="https://github.com/user-attachments/assets/ddff87c5-0211-4786-aed4-b60efae7db15" />


#### Report Capacitance Per Stage


It reports timing paths with four-digit precision and displays the net capacitance at each stage, making it easier to identify high-capacitance nodes that could impact delay. Run the following command in the OpenSTA Docker Container:

```
% report_checks -digits 4 -fields capacitance
```

<img width="1219" height="726" alt="Screenshot from 2025-10-18 22-09-21" src="https://github.com/user-attachments/assets/0002a7fb-b3f7-4052-bd43-3370260a83d1" />


#### Detailed Timing Analysis with Capacitance, Slew, Inputs, and Fanout

This report provides detailed timing information for each path, including net capacitance, signal slew, input pin delays, and fanout, helping identify potential bottlenecks in the design. Run the following command in the OpenSTA Docker Container:

```
% report_checks -digits 4 -fields [list capacitance slew input_pins fanout]
```
<img width="1219" height="731" alt="Screenshot from 2025-10-18 22-09-40" src="https://github.com/user-attachments/assets/9e07e2f7-7b31-4e1e-bf16-81d87f5eaeb2" />


#### Total and Component Power Reporting

The ```report_power``` command performs static power analysis using propagated or annotated pin activities along with Liberty power models.

It provides a breakdown of internal, switching, leakage, and total power. Power consumption is reported separately for combinational logic, sequential elements, macros, and pad groups, with all values expressed in watts. Run the following command in the OpenSTA Docker Container:

```
% report_power
```

<img width="811" height="382" alt="Screenshot from 2025-10-18 22-10-15" src="https://github.com/user-attachments/assets/7654feb4-dfa4-40c8-9f16-8d34373a587a" />


#### Reporting Pulse Width Checks

The ```report_pulse_width_checks``` command generates minimum pulse width checks for pins in the clock network.

If no specific pins are provided, the command reports pulse width checks for all clock network pins by default. Run the following command in the OpenSTA Docker Container:

```
% report_pulse_width_checks
```

<img width="811" height="310" alt="Screenshot from 2025-10-18 22-11-10" src="https://github.com/user-attachments/assets/9b7fc451-247d-4c86-bd55-ae29d9225768" />


#### Reporting Units

The ```report_units``` command displays the measurement units used for command arguments and reported values throughout the design analysis. Run the following command in the OpenSTA Docker Container:

```
% report_units
```

<img width="811" height="285" alt="Screenshot from 2025-10-18 22-11-20" src="https://github.com/user-attachments/assets/a0da1fb8-be95-4654-8ab3-9f0a120c78e2" />


### Automating Timing Analysis with a TCL Script

You can automate the OpenSTA timing flow by writing commands into a .tcl script and executing it from the OpenSTA shell. For example, the script min_max_delays.tcl could include:

```
# Load Liberty timing libraries for max (setup) and min (hold) analysis
read_liberty -max /data/VLSI/VSDBabySoC/OpenSTA/examples/nangate45_slow.lib.gz
read_liberty -min /data/VLSI/VSDBabySoC/OpenSTA/examples/nangate45_fast.lib.gz

# Load the gate-level Verilog netlist
read_verilog /data/VLSI/VSDBabySoC/OpenSTA/examples/example1.v

# Link the top-level module
link_design top

# Define a 10 ns clock for clk1, clk2, and clk3, and set input delays
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}

# Generate a timing report including both min (hold) and max (setup) paths
report_checks -path_delay min_max
```

<img width="1219" height="633" alt="Screenshot from 2025-10-19 10-17-08" src="https://github.com/user-attachments/assets/68c4eed8-a307-462b-a657-ad77abb7c912" />

<img width="1219" height="667" alt="Screenshot from 2025-10-19 10-17-20" src="https://github.com/user-attachments/assets/cf7a1fa2-6780-469d-9134-99b11e72c8d3" />

<img width="1219" height="712" alt="Screenshot from 2025-10-19 10-17-29" src="https://github.com/user-attachments/assets/75611bf2-84e0-4739-a8de-c05dd2f6ce15" />


### VSDBabySoC Basic Timing Analysis

**Prepare The Required Files**

Before starting static timing analysis on the VSDBabySoC design, organize the necessary files into appropriate directories:

```
# In OpenSTA/examples, create a directory for Liberty timing libraries
cd ~/OpenSTA/examples
mkdir -p timing_libs/
ls timing_libs/
```

<img width="1192" height="117" alt="Screenshot from 2025-10-20 15-14-00" src="https://github.com/user-attachments/assets/49fbb69f-21c5-4d4a-aff4-94c7a9865466" />


```
# In OpenSTA/examples/BabySoC, create a directory for the synthesized netlist and constraints
mkdir -p BabySoC
ls BabySoC/
```


<img width="920" height="157" alt="Screenshot from 2025-10-20 15-16-38" src="https://github.com/user-attachments/assets/6ab8a1c5-48b2-415b-9e66-b0f4b06f4504" />


**The necessary files for timing analysis are:**

- *Standard cell library:* ```sky130_fd_sc_hd__tt_025C_1v80.lib```

- *IP-specific timing libraries:* ```avsdpll.lib``` and ```avsddac.lib```

- *Synthesized gate-level netlist:* ```vsdbabysoc.synth.v```

- *Design timing constraints:* ```vsdbabysoc_synthesis.sdc```



To perform a complete **min/max timing analysis** on the VSDBabySoC design, you can use the following TCL script ```vsdbabysoc_min_max_delays.tcl```:

```
# Load the standard cell library for minimum (hold) timing analysis
read_liberty -min /data/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib

# Load the standard cell library for maximum (setup) timing analysis
read_liberty -max /data/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib

# Load the IP-specific library for minimum timing
read_liberty -min /data/OpenSTA/examples/timing_libs/avsdpll.lib

# Load the IP-specific library for maximum timing
read_liberty -max /data/OpenSTA/examples/timing_libs/avsdpll.lib

# Read the synthesized gate-level Verilog netlist
read_verilog /data/OpenSTA/examples/BabySoC/vsdbabysoc.synth.v

# Link the top-level SoC design
link_design vsdbabysoc

# Load the design constraints file (SDC)
read_sdc /data/OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc

# Generate a full timing report including both min (hold) and max (setup) paths
report_checks
```


### VSDBabySoC PVT Corner Analysis (Post-Synthesis Timing)

Static Timing Analysis (STA) is performed across multiple **PVT (Process-Voltage-Temperature) corners** to verify that the design meets timing requirements under different operating conditions.

| Timing Type                        | Critical Corners                              | Description                          |
|-----------------------------------|-----------------------------------------------|--------------------------------------|
| **Worst Max Path (Setup-critical)** | `ss_LowTemp_LowVolt`<br>`ss_HighTemp_LowVolt` | Represents the slowest operating conditions |
| **Worst Min Path (Hold-critical)**  | `ff_LowTemp_HighVolt`<br>`ff_HighTemp_HighVolt` | Represents the fastest operating conditions |


All the timing libraries necessary for the analysis can be downloaded from this repository [here](https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing)


To perform **Static Timing Analysis (STA)** across all available PVT corners for VSDBabySoC, you can use the following TCL script ```sta_across_pvt.tcl```:

```
# List of Sky130 Liberty files corresponding to different PVT corners
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

# Load IP-specific Liberty libraries
read_liberty /OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty /OpenSTA/examples/timing_libs/avsddac.lib

# Loop over all PVT corner Liberty files
for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {

    # Load the current Liberty file for this PVT corner
    read_liberty /OpenSTA/examples/timing_libs/$list_of_lib_files($i)

    # Load the synthesized gate-level netlist
    read_verilog /OpenSTA/examples/BabySoC/vsdbabysoc.synth.v

    # Link the top-level SoC design
    link_design vsdbabysoc

    # Verify the current design
    current_design

    # Read the design constraints (SDC)
    read_sdc /OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc

    # Perform setup checks with verbose output
    check_setup -verbose

    # Generate full min/max timing report including nets, cap, slew, input pins, fanout
    report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /OpenSTA/examples/BabySoC/STA_OUTPUT/min_max_$list_of_lib_files($i).txt

    # Record worst max (setup) slack
    exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt
    report_worst_slack -max -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt

    # Record worst min (hold) slack
    exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt
    report_worst_slack -min -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt

    # Record Total Negative Slack (TNS)
    exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt
    report_tns -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt

    # Record Worst Negative Slack (WNS)
    exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
    report_wns -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
}
```

- ```report_worst_slack -max``` reports the **worst setup slack** for the current PVT corner, showing the most negative setup slack (WNS) in the design.

- ```report_worst_slack -min``` reports the **worst hold slack**, indicating the most negative hold slack in the design for the current corner.

- ```report_tns``` prints the **Total Negative Slack (TNS)**, which is the sum of all negative slacks across violating paths, giving a measure of how widespread timing violations are.

- ```report_wns``` prints the **Worst Negative Slack (WNS)**, showing the single most timing-critical path and indicating the severity of the worst violation.


To run the STA across all PVT corners, execute the TCL script inside the OpenSTA Docker container:

```
docker run -it -v $HOME:/data opensta /data/OpenSTA/examples/BabySoC/sta_across_pvt.tcl
```


<img width="1184" height="730" alt="Screenshot from 2025-10-20 15-32-19" src="https://github.com/user-attachments/assets/c737bbec-0fc5-4ba4-b2ac-75cf0bfa1de8" />


After the script finishes, all generated timing reports can be found in the STA_OUTPUT directory:

```
sai-akhil-cheruvu@sai-akhil-cheruvu-VirtualBox:~/OpenSTA/examples/BabySoC/STA_OUTPUT$ ls
min_max_sky130_fd_sc_hd__ff_100C_1v65.lib.txt
min_max_sky130_fd_sc_hd__ff_100C_1v95.lib.txt
min_max_sky130_fd_sc_hd__ff_n40C_1v56.lib.txt
min_max_sky130_fd_sc_hd__ff_n40C_1v65.lib.txt
min_max_sky130_fd_sc_hd__ff_n40C_1v76.lib.txt
min_max_sky130_fd_sc_hd__ss_100C_1v40.lib.txt
min_max_sky130_fd_sc_hd__ss_100C_1v60.lib.txt
min_max_sky130_fd_sc_hd__ss_n40C_1v28.lib.txt
min_max_sky130_fd_sc_hd__ss_n40C_1v35.lib.txt
min_max_sky130_fd_sc_hd__ss_n40C_1v40.lib.txt
min_max_sky130_fd_sc_hd__ss_n40C_1v44.lib.txt
min_max_sky130_fd_sc_hd__ss_n40C_1v76.lib.txt
min_max_sky130_fd_sc_hd__tt_025C_1v80.lib.txt
sta_worst_max_slack.txt
sta_worst_min_slack.txt
sta_tns.txt
sta_wns.txt
```


- ```min_max_<lib>.txt``` contains a detailed timing report for both setup and hold paths for each PVT corner.

- ```sta_worst_max_slack.txt``` lists the worst-case setup slack values across all corners.

- ```sta_worst_min_slack.txt``` lists the worst-case hold slack values across all corners.

- ```sta_tns.txt``` reports the Total Negative Slack (TNS) across all corners.

- ```sta_wns.txt``` reports the Worst Negative Slack (WNS) across all corners.


## Post-Synthesis STA Overview Across PVT Corners

To evaluate the timing robustness of VSDBabySoC, static timing analysis (STA) was performed across 13 different PVT (Process-Voltage-Temperature) corners using OpenSTA.

From the analysis, key timing metrics were collected for each corner, including:

- **Worst Setup Slack** – the most critical setup path delay.

- **Worst Hold Slack** – the most critical hold path timing.

- **Worst Negative Slack (WNS)** – the single most timing-violating path.

- **Total Negative Slack (TNS)** – the sum of all violating paths, showing the overall timing margin.


<img width="1186" height="387" alt="image" src="https://github.com/user-attachments/assets/47983252-a37f-4ede-8383-98c21fdd7005" />


### 1. Plot Of Total Negative Slack (TNS):


<img width="3845" height="1813" alt="image" src="https://github.com/user-attachments/assets/3db3b740-4133-47aa-bcab-fdf93ddf4128" />


### 2. Plot Of Worst Negative Slack (WNS):


<img width="4508" height="1631" alt="image" src="https://github.com/user-attachments/assets/40f6e625-5e60-41d3-b1a5-1bf4d3850142" />



### 3. Plot Of Worst Hold Slack:


<img width="3819" height="1842" alt="image" src="https://github.com/user-attachments/assets/0744c768-1599-486a-8a0c-2d028b16d749" />



### 4. Plot Of Worst Setup Slack:


<img width="4402" height="1758" alt="image" src="https://github.com/user-attachments/assets/29cfaf51-bd6d-40b7-9eb6-5aec3b7d7dba" />



## Summary

This repository demonstrates the post-synthesis static timing analysis (STA) workflow for the BabySoC design using OpenSTA, fully automated via Tcl scripts and Docker. The project is structured to include:

- **Design Files:** Gate-level Verilog netlist and SDC constraints for the BabySoC design.
- **Timing Libraries:** Standard cell libraries required for min/max delay analysis. Currently, `sky130_fd_sc_hd__tt_025C_1v80.lib` is used for timing calculations.
- **Automation:** Tcl scripts (`vsdbabysoc_min_max_delays.tcl`) automate library reading, netlist linking, SDC application, and generation of timing reports.
- **Docker Integration:** OpenSTA runs inside a Docker container, ensuring environment consistency and easy reproducibility.

### Key Outcomes of Timing Analysis

1. **Timing Verification:**  
   - Setup and hold paths were checked across all corners to ensure timing requirements are met under slow and fast operating conditions.

2. **Min/Max Path Analysis:**  
   - Detailed min/max timing reports were generated for each corner, highlighting critical paths and slack margins.

3. **Slack Metrics:**  
   - Worst-case setup slack, hold slack, Worst Negative Slack (WNS), and Total Negative Slack (TNS) were recorded to evaluate timing robustness.

4. **PVT Coverage:**  
   - Analysis was performed across 13 different PVT (Process-Voltage-Temperature) conditions, confirming the design maintains functional timing under varying operating environments.

5. **Automated and Reproducible Flow:**  
   - The use of Tcl scripts and Docker-enabled OpenSTA allows the entire STA flow to be run consistently across different machines without manual intervention.

### Conclusion

This setup provides a reproducible and automated workflow for timing verification of the BabySoC design, ensuring robust performance under multiple operating conditions and highlighting the design’s timing-critical paths. The repository structure and scripts are designed to be easily extensible for future designs or additional timing libraries.
