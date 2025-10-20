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




### Analysing Report Outcomes


The analysis is performed on the Verilog netlist file ```example1.v``` :



<img width="865" height="335" alt="Screenshot from 2025-10-20 13-24-17" src="https://github.com/user-attachments/assets/070bfa4f-b2f6-4030-9012-617b518ff396" />
