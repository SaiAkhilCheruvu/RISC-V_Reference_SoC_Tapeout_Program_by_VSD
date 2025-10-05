# Post-Synthesis GLS Of VSDBabySoC


## What is Gate Level Simulation (GLS)

Gate-level simulation models a digital circuit using actual logic gates and their interconnections, including propagation delays, glitches, and timing behavior. Unlike RTL simulation, it shows how the design will behave post-synthesis, reflecting the real hardware more accurately.


| Feature                         | RTL Simulation                                    | Gate-Level Simulation                                     |
| ------------------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| **Abstraction Level**           | High-level, behavioral description of the circuit | Low-level, uses actual logic gates and interconnections   |
| **Purpose**                     | Verify design functionality early                 | Verify post-synthesis design correctness and timing       |
| **Timing Accuracy**             | Ignores gate delays                               | Includes propagation delays, glitches, and timing effects |
| **Simulation Speed**            | Faster                                            | Slower due to detailed gate-level modeling                |
| **Post-Synthesis Verification** | No                                                | Yes                                                       |
| **Glitch/Hazard Detection**     | Limited                                           | Can detect hazards and glitches due to gate delays        |
| **Use Case**                    | Early functional verification, design exploration | Final verification before fabrication                     |




## Why Perform GLS?

GLS is performed to:

- Verify correctness after synthesis.

- Check timing issues like setup/hold violations or glitches.

- Ensure the circuit works reliably under real hardware conditions before fabrication.



## Highlights Of GLS

### 1. Timing Concepts

- **Propagation delay:** Time for a signal to travel through a gate.

- **Setup time:** Minimum time before a clock edge that data must be stable.

- **Hold time:** Minimum time after a clock edge that data must remain stable.

- **Clock skew & timing violations:** Differences in clock arrival can cause setup/hold violations.


### 2. Synthesis and Optimization

- Converts RTL into gates for hardware implementation.

- **Optimizations:** Improve speed, reduce area, or save power.


### 3. Glitch Analysis / Hazard Detection

- **Static hazards:** Unwanted glitches on single transitions.

- **Dynamic hazards:** Multiple unintended transitions due to delays.

- Gate-level simulation is key to detecting hazards missed in RTL simulation.


### 4. Testbench Design

- Includes stimulus generation, assertions, and waveform checking.

- Gate-level simulation often requires more detailed testbenches than RTL.


### 5. Power and Area Estimation

- Gate-level simulation helps estimate real hardware power consumption and circuit area.


### 6. Common Tools and Flow

- **Simulators:** ModelSim, VCS, QuestaSim.

- **Synthesis tools:** Synopsys Design Compiler, Cadence Genus.

- **Typical flow:** RTL design → Synthesis → Gate-level netlist → Gate-level simulation → Timing/power analysis.


## Step-Wise GLS Implementation For VSDBabySoC

### Step 1: Launch Yosys

```
yosys

# Start the Yosys synthesis tool interactive shell.
```


### Step 2: Load RTL Design Files

```
yosys> read_verilog /home/sai-akhil-cheruvu/VSDBabySoC/src/module/vsdbabysoc.v

yosys> read_verilog -I /home/sai-akhil-cheruvu/VSDBabySoC/src/include \
    /home/sai-akhil-cheruvu/VSDBabySoC/output/compiled_tlv/rvmyth.v

yosys> read_verilog -I /home/sai-akhil-cheruvu/VSDBabySoC/src/include \
    /home/sai-akhil-cheruvu/VSDBabySoC/src/module/clk_gate.v

# Import RTL source files: RISC-V core, clock gating, and top-level SoC.

```


![Screenshot from 2025-10-05 11-40-08](https://github.com/user-attachments/assets/68738651-cb9f-42a2-a749-37fdb09ac47f)
![Screenshot from 2025-10-05 11-40-15](https://github.com/user-attachments/assets/7ed1be32-9efc-464d-a261-6c2005fd6bbf)




### Step 3: Load Technology Libraries

```
yosys> read_liberty -lib /home/sai-akhil-cheruvu/VSDBabySoC/src/lib/avsdpll.lib

yosys> read_liberty -lib /home/sai-akhil-cheruvu/VSDBabySoC/src/lib/avsddac.lib

yosys> read_liberty -lib /home/sai-akhil-cheruvu/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Load cell libraries (PLL, DAC, and standard cell library for Sky130).

```

<img width="828" height="542" alt="Screenshot from 2025-10-05 21-59-42" src="https://github.com/user-attachments/assets/235d4598-52d9-42dd-90cf-e7d8a2cc2593" />




### Step 4: Run Synthesis

```
yosys> synth -top vsdbabysoc

# Run high-level synthesis using vsdbabysoc as the top module.

yosys> dfflibmap -liberty /home/sai-akhil-cheruvu/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Map flip-flops to standard cells.
```

<img width="988" height="697" alt="Screenshot from 2025-10-05 22-03-45" src="https://github.com/user-attachments/assets/ea0310bc-55b3-4e95-ba83-7a5c2057ecf2" />

<img width="990" height="381" alt="Screenshot from 2025-10-05 13-10-36" src="https://github.com/user-attachments/assets/96a8c258-4b19-481d-82e3-968d167c40eb" />

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-10-45" src="https://github.com/user-attachments/assets/460a6622-9c7a-476b-8127-b0a0742c0077" />

<img width="990" height="366" alt="Screenshot from 2025-10-05 13-11-07" src="https://github.com/user-attachments/assets/0e483a1c-f0ff-4087-927e-d0f63bc2bf5c" />

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-11-12" src="https://github.com/user-attachments/assets/15681366-4cb3-4684-947e-f23605d06999" />

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-13-32" src="https://github.com/user-attachments/assets/fc763181-e15e-4c0a-953f-dc247de01007" />



### Step 5: Optimize and Technology Map

```
yosys> opt

# Perform logic optimization.

yosys> abc -liberty /home/sai-akhil-cheruvu/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib \
    -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}

# Optimize and map logic to the standard cell library using ABC.
```

<img width="1147" height="762" alt="Screenshot from 2025-10-05 22-08-27" src="https://github.com/user-attachments/assets/8b781c1e-ebe8-4006-ae9a-9374c6ece519" />

<img width="1091" height="139" alt="Screenshot from 2025-10-05 22-12-22" src="https://github.com/user-attachments/assets/6e7d65a2-5d72-4799-8c77-d4103df273f6" />

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-51-57" src="https://github.com/user-attachments/assets/b7204df1-76f2-4e0c-aba8-5b275f420911" />

<img width="940" height="635" alt="Screenshot from 2025-10-05 13-52-30" src="https://github.com/user-attachments/assets/fef29e1a-fb21-4812-a166-ce14fc1924d9" />



### Step 6: Post-Processing and Cleanup

```
yosys> flatten

# Flatten hierarchy into a single module.

yosys> setundef -zero

# Replace undefined (X) states with logic 0.

yosys> clean -purge

# Remove unused or redundant logic.

yosys> rename -enumerate

# Rename nets and modules with unique identifiers.
```

<img width="830" height="476" alt="Screenshot from 2025-10-05 13-55-15" src="https://github.com/user-attachments/assets/33448f15-cd49-4ddf-930e-3324bcafcccb" />



### Step 7: Analyze and Write Output

```
yosys> stat

# Display design statistics (gates, cells, etc.).

yosys> write_verilog -noattr /home/sai-akhil-cheruvu/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v

# Export final synthesized netlist.
```

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-56-22" src="https://github.com/user-attachments/assets/4bf10b95-a0d3-4419-a604-e808e4ea720e" />

<img width="1280" height="800" alt="Screenshot from 2025-10-05 13-56-30" src="https://github.com/user-attachments/assets/8e585f99-ad47-4ad7-82a5-200fc67bdb01" />

<img width="1093" height="298" alt="Screenshot from 2025-10-05 14-22-01" src="https://github.com/user-attachments/assets/6874e8b6-496e-4665-8b14-db477dfa9bcf" />


### Step 8: Compile the Simulation

```
iverilog -o /home/sai-akhil-cheruvu/VSDBabySoC/output/post_synth_sim/post_synth_sim.out \
  -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 \
  -I /home/sai-akhil-cheruvu/VSDBabySoC/src/include \
  -I /home/sai-akhil-cheruvu/VSDBabySoC/src/module \
  /home/sai-akhil-cheruvu/VSDBabySoC/src/module/testbench.v

# Compile the testbench with synthesis defines and generate simulation binary.

```

<img width="1091" height="145" alt="Screenshot from 2025-10-05 22-18-13" src="https://github.com/user-attachments/assets/5878d4ee-9c05-410a-a45a-95d5322985b6" />




### Step 9: Go to the Simulation Directory, run the simulation and view the results using GTKWave

```
cd /home/sai-akhil-cheruvu/VSDBabySoC/output/post_synth_sim/

# Move into the directory containing simulation files.

./post_synth_sim.out

# Execute the compiled simulation binary (produces post_synth_sim.vcd)

gtkwave post_synth_sim.vcd

# Open waveform dump in GTKWave for analysis.

```

<img width="1093" height="324" alt="Screenshot from 2025-10-05 15-02-49" src="https://github.com/user-attachments/assets/7b18b367-3ee3-4271-a506-8a09855dc73d" />


<img width="1280" height="800" alt="Screenshot from 2025-10-05 15-03-17" src="https://github.com/user-attachments/assets/c9a24769-4cc9-4e75-a4a5-69d9c57ef2db" />

