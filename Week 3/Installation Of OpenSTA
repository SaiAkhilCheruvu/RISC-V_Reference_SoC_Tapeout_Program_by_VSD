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

