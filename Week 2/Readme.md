# Week 2


## 1. Introduction to the VSDBabySoC

VSDBabySoC is a compact yet capable RISC-V based System-on-Chip (SoC) designed primarily for educational and testing purposes. Its main goals are to integrate and test three open-source IP cores together for the first time and to calibrate the analog components effectively.

**Key components of VSDBabySoC:**

- **RVMYTH Microprocessor:** Core processing unit implementing RISC-V instruction set

- **8x Phase-Locked Loop (PLL):** Generates a stable system clock

- **10-bit DAC:** Interfaces with analog devices, illustrating mixed-signal integration


<img width="2270" height="1260" alt="image" src="https://github.com/user-attachments/assets/4b95cb9a-a25a-4acc-b931-0fe1c9b4005d" />



This small-scale SoC provides a realistic but approachable platform for students and developers to learn SoC design, functional modeling, and verification methodologies.

## 2. Overview of System-on-Chip (SoC) Design


### 2.1 Defining SoCs and Their Core Principles

A System-on-Chip (SoC) is an advanced form of integrated circuit that brings together multiple subsystems—processors, memory, communication blocks, and peripherals—onto a single silicon die. This consolidation transforms what once required several discrete chips into a unified solution, enabling high performance, low power operation, and compact designs.

The driving principle is functional convergence, where varied computational and control capabilities coexist in one silicon package. Compared to multi-chip assemblies, SoCs provide:

- **Simplified System Design:** Fewer discrete interconnections and components
  
- **Improved Efficiency:** Shorter signal delays and minimized parasitic losses
  
- **Lower Power Consumption:** Optimized power usage across functional domains
  
- **Reduced Cost:** Streamlined manufacturing and assembly process
  
- **Higher Reliability:** Fewer physical interconnects reduce failure points
  

### 2.2 Evolutionary Path and Market Influences

The SoC’s rapid development can be traced to technological enablers and growing market needs.

- **Technology Enablers:** Shrinking process nodes (65nm, 45nm, 28nm and below), higher transistor density per Moore’s Law, enhanced EDA (Electronic Design Automation) tools, and reusable IP blocks.
  
- **Market Drivers:** Longer battery life for mobile devices, miniaturization for IoT products, cost constraints in consumer electronics, and stringent automotive safety standards.

### 2.3 Hierarchies in Design Abstraction

Designing an SoC requires working across abstraction layers:

- **System Level:** High-level architecture and algorithms
 
- **Behavioral:** Functionality modeling without hardware mapping

- **RTL (Register-Transfer Level):** Precise timing and control flow

- **Gate Level:** Boolean logic representation

- **Transistor Level:** Device-specific implementation

- **Layout:** Geometric design for fabrication


## 3. Structural Elements of SoC Architecture


### 3.1 Computational Engines

Modern SoCs feature a variety of processors tailored to application domains:

- **General CPUs:** Application processors, real-time processors, microcontrollers
  
- **Specialized Units:** GPUs for graphics and parallel tasks, DSPs for signal processing, NPUs for AI workloads


### 3.2 Memory Framework

Performance hinges on memory subsystem design:

- **On-Chip:** Multi-level caches (L1–L3), register files, tightly coupled memories
  
- **Off-Chip Interfaces:** DRAM and Flash controllers, MMUs for address translation


### 3.3 Data Interconnects

Interconnects dictate efficiency in communication:

- **Bus Models:** AHB for high performance, APB for peripherals, AXI for advanced system designs
  
- **Network-on-Chip:** Scalable packet-based communication, QoS support, and power-conscious routing

### 3.4 I/O and Peripheral Units

Connectivity makes SoCs versatile:

- **Communication:** USB, Ethernet, wireless interfaces (WiFi, Bluetooth, cellular)
  
- **Sensors and Actuators:** ADCs, DACs, PWM drivers

- **User Interfaces:** Display controllers, touch inputs, audio codecs


## 4. VSDBabySoC: A Learning-Oriented SoC


### 4.1 Purpose and Pedagogy

The VSDBabySoC is crafted as a teaching aid to simplify yet realistically represent SoC design. It emphasizes:

- **Balanced Complexity:** Enough detail to reflect real systems without overwhelming learners
  
- **Industry Relevance:** Uses recognized protocols and standard interfaces

- **Stepwise Learning:** Modular design to help grasp concepts progressively


### 4.2 Core Modules

- **RVMYTH Processor (RISC-V based):** Demonstrates instruction pipelines, ALU operations, register file usage, and memory interactions

- **PLL (Phase-Locked Loop):** Introduces clock multiplication, jitter reduction, and synchronization concepts

- **10-bit DAC:** Highlights digital-to-analog interfacing, accuracy challenges, and mixed-signal integration


### 4.3 Integration Blueprint

- **Hierarchical Design:** From module-level building blocks to full system integration

- **Clock and Reset Strategies:** Synchronous domain management and initialization

- **Signal Integrity:** Careful routing, analog-digital isolation, and power decoupling


## 5. Functional Modeling and Verification

### 5.1 Abstraction for Functional Modeling

Functional modeling validates system intent before detailed implementation, supporting:

- Algorithm testing

- Architecture exploration

- Early software development

- Performance analysis
  

### 5.2 Verification Techniques

- **Simulation-Based:** Directed tests, random coverage-driven methods, assertion checks
  
- **Formal Verification:** Property proofs, equivalence checks, model checking


### 5.3 Testbench Infrastructure

Includes stimulus generation (corner cases, compliance testing) and response validation (coverage analysis, reference model comparisons).


## 6. Verification and Validation Practices

- **Unit-Level:** Testing individual modules and interfaces
  
- **Integration-Level:** Ensuring component collaboration works as intended
  
- **System-Level:** Running end-to-end functional and stress tests

Tools include Icarus Verilog, ModelSim, VCS for simulation, and GTKWave for waveform inspection.


## 7. Integration and Interconnect Strategies

- **Challenges:** Timing closure, power optimization, managing clock domains

- **Interfaces:** AXI/AHB buses, GPIOs, interrupts, and custom protocols for performance-tuned applications


## 8. Mixed-Signal Considerations

- **Noise Isolation:** Guarding analog blocks from digital switching noise

- **Power Supplies:** Separate domains for analog and digital, decoupling strategies

- **Signal Integrity:** Termination, crosstalk control, and high-speed data routing


## 9. Educational Outcomes

Learners using VSDBabySoC build:

- **Technical Expertise:** HDL proficiency, EDA tools, debugging, and verification strategies

- **Architectural Insight:** Trade-off analysis and performance optimization

- **Professional Competence:** Documentation, teamwork, and project management


## 10. Industry Relevance

### 10.1 Current Trends

- Open-source hardware ecosystems (RISC-V adoption, open toolchains)

- AI/ML acceleration at the edge

- System integration for IoT and automotive industries


### 10.2 Emerging Perspective: Chiplet-Based SoCs

A new frontier in SoC design is chiplet architecture, where multiple smaller dies (chiplets) are integrated within a single package. Instead of one monolithic SoC, different chiplets (e.g., CPU, GPU, memory, I/O) are fabricated separately and then connected using high-speed interconnects.

Advantages include:

- **Yield Improvement:** Smaller dies reduce defect-related waste

- **Flexibility:** Ability to mix different process nodes (e.g., CPU on 5nm, analog on 28nm)

- **Scalability:** Modular addition of specialized accelerators

- **Cost Efficiency:** Optimized use of manufacturing resources

Chiplet-based integration complements traditional SoC design by overcoming scaling limitations while maintaining performance and power efficiency.


### 10.3 Career Preparation and Professional Development

Students gain industry-aligned skills in:

- EDA tool usage (Synopsys, Cadence, Mentor Graphics)
  
- Protocol knowledge (AXI, PCIe, USB)

- Verification expertise (UVM, SVA)

- Mixed-signal awareness
  

Career roles span ASIC/FPGA design, verification, integration, and technical leadership.

## 11. Conclusion and Outlook

VSDBabySoC serves as an effective bridge between academic concepts and industry practices. Learners gain:

- A complete picture of SoC design and verification

- Hands-on exposure to real-world tools and protocols

- System-level thinking applicable to professional challenges


Future Pathways include advanced verification (UVM, formal), physical design techniques, memory and interface optimization, and specialized domains like chiplet integration, security, and low-power design.

This foundation equips students to adapt to the fast-evolving semiconductor landscape, fostering both technical mastery and professional growth.


## VSDBabySoC Simulation

In this section, we provide a step-by-step walkthrough of VSDBabySoC simulation, covering both pre-synthesis and post-synthesis stages. The goal is to observe the interaction between the RVMYTH digital core and the DAC peripheral, by adjusting digital output values and monitoring the resulting changes on the DAC output.


### Pre-Synthesis Simulation

The pre-synthesis simulation allows us to verify the functional behavior of the VSDBabySoC before any synthesis or hardware mapping. In this stage, the digital RVMYTH core and DAC model are simulated together, enabling observation of signal interactions and output behavior.

**Step 1 : Install The Required Packages**

```
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
$ sudo chmod 666 /var/run/docker.sock
$ cd ~
$ pip3 install pyyaml click sandpiper-saas
```

**Step 2 : Clone the repository into your chosen directory**

```
$ cd ~
$ git clone https://github.com/manili/VSDBabySoC.git
```


**Step 3 : Generate the pre-synthesis simulation file (```pre_synth_sim.vcd```)**

```
$ cd VSDBabySoC
$ make pre_synth_sim
```

The simulation result (```pre_synth_sim.vcd```) will be saved in the ```output/pre_synth_sim/``` directory


**Step 4 : View the waveforms using GTKWave**

```
$ gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

The two key signals to observe are ```CLK```, provided by the ```PLL```, and ```OUT```, which represents the DAC model’s output. This illustrates the final result of the pre-synthesis simulation.


<img width="1280" height="800" alt="Screenshot from 2025-10-03 10-37-50" src="https://github.com/user-attachments/assets/521eaec2-2c6e-44ff-ab6e-8e6e3606a5c6" />


In this waveform, the following signals can be observed:

```CLK``` – The clock input to the RVMYTH core, originally generated by the PLL.

```reset``` – The reset input to the RVMYTH core, coming from an external source.

```OUT (wire)``` – The output signal of the VSDBabySoC module. It originates from the DAC, but due to simulation limitations, it behaves as a digital signal rather than analog.

```RV_TO_DAC[9:0]``` – The 10-bit output from RVMYTH register #17, serving as the input to the DAC.

```OUT (real)``` – A real-valued wire from the DAC module that can simulate analog behavior.

**Important Note:** The synthesis process does not support real-valued signals, so \vsdbabysoc.OUT must use a standard wire type. In Icarus Verilog, this wire behaves digitally, which prevents observing true analog output. To see analog-like behavior, monitor \dac.OUT, which is a real-valued signal from the DAC.


### Post-Synthesis Simulation

The post-synthesis simulation allows us to verify the VSDBabySoC functionality after synthesis, reflecting how the design would behave when implemented in hardware. This stage ensures that the synthesized netlist matches the intended logic and that timing and functional correctness are preserved.

To perform post-synthesis simulation, first synthesize the VSDBabySoC design to generate the hardware-mapped netlist:

```
$ cd ~/VSDBabySoC
$ make synth
```

**Step 1: Run the post-synthesis simulation**

```
cd ~/VSDBabySoC
make post_synth_sim
```

**Step 2: Run the post-synthesis simulation**


