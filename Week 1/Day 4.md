# Day 4

Day 4 focused on understanding the importance of gate-level simulation (GLS), the differences between blocking and non-blocking statements in Verilog, and how mistakes in RTL coding styles can result in simulation-synthesis mismatches.


## What Is GLS?

Gate Level Simulation (GLS) is running a testbench on the gate-level netlist (post-synthesis representation). The netlist is a logical equivalent of the RTL, so the same testbench can be used to verify the design at this stage.


<img width="1280" height="800" alt="Screenshot from 2025-09-27 23-46-12" src="https://github.com/user-attachments/assets/85034e58-51a8-4fbe-8344-8e80a1a247f1" />


This diagram shows the Gate Level Simulation (GLS) flow using Icarus Verilog (iverilog), where the design, gate-level models, and testbench are compiled with iverilog to generate a VCD file. The VCD file can then be viewed in GTKWave for waveform analysis and timing validation, especially if the gate-level models are delay-annotated.

## Why Perform GLS?

GLS ensures the timing correctness of the synthesized design, verifying that logical functionality remains intact after synthesis optimizations, and flags timing-related issues missed by RTL simulation.


## When Is GLS Performed?

It is performed after synthesis, before place-and-route, to validate post-synthesis design behavior and timing.


## Types Of GLS?

- Functional GLS: Simulates gate-level netlist without timing delays to verify functional equivalence with RTL.

- Timing-aware GLS: Includes timing delays annotated from Standard Delay Format (SDF) files to check actual timing and detect setup/hold violations and glitches.




## Synthesis-Simulation Mismatch

A synthesis-simulation mismatch occurs when the behavior seen in RTL simulation does not match the results from gate-level simulation or actual hardware. Common causes include:

- **Non-synthesizable code:** Using delays, initial blocks, or other constructs that synthesis tools ignore.

- **Ambiguous or incomplete logic:** Missing else branches or improper sensitivity lists can lead to unintended latches or incorrect hardware.

- **Tool interpretation differences:** Simulation and synthesis tools may handle ambiguous RTL differently.


## Blocking And Non-Blocking Statements In Verilog

### 🔹 Blocking Assignments (=)

#### How they work:

- Statements execute sequentially inside the procedural block.

- The left-hand side (LHS) is updated immediately before the next statement runs.

- Used mostly for combinational logic descriptions.

*Example:*

```
always @(*) begin
  y = a & b;   // y updated right away
  z = y | c;   // z sees the new y
end
```
Here z is computed using the new value of y because blocking assignments update LHS immediately. This models the way combinational logic should evaluate step by step.

###🔹 Non-Blocking Assignments (<=)

#### How they work:

- RHS expressions are evaluated in order, but LHS updates are scheduled to happen at the end of the current time step.

- Effectively, all LHS updates in the block happen “together” at the clock edge.

- Used mostly for sequential (clocked) logic so registers update in parallel.


*Example:*

```
always @(posedge clk) begin
  q1 <= d;
  q2 <= q1;
end
```


At a rising clock edge, both RHS values (d and the old q1) are sampled first. Only after all are sampled do the LHS registers update. So q1 gets d immediately, but q2 gets the previous q1 — making this a proper shift register across clock cycles.



| Feature / Aspect              | Blocking (`=`)                                    | Non-Blocking (`<=`)                                           |
| ----------------------------- | ------------------------------------------------- | ------------------------------------------------------------- |
| **Execution Order**           | Sequential, one after another                     | RHS evaluated sequentially, LHS updated together              |
| **LHS Update Timing**         | Immediate (before next statement)                 | Deferred until end of time step                               |
| **Behavior Within Time Step** | Later statements see updated values of earlier    | Later statements see **old** values until update              |
| **Best For**                  | Combinational logic (always @(*))                 | Sequential/clocked logic (always @(posedge clk))              |
| **Common Pitfall**            | Not suitable for multi-register synchronous logic | Using blocking in sequential blocks can cause race conditions |


## Demonstrations

### Demonstration 1 - Ternary Operator Multiplexer

Verilog Program:

```
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```


#### Synthesis Of Ternary Operator Multiplexer:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

<img width="1280" height="758" alt="1" src="https://github.com/user-attachments/assets/5f375c3b-9a0f-4a7a-a96a-30535f209b91" />



#### Simulation Of Ternary Operator Multiplexer:

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```



<img width="1280" height="754" alt="2" src="https://github.com/user-attachments/assets/65b9e082-5f17-4a42-a973-999f47b83a10" />



Use the following command run a gate-level simulation (GLS):

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd

```


#### GLS Of Ternary Operator Multiplexer:

<img width="1280" height="762" alt="3" src="https://github.com/user-attachments/assets/191d989d-672d-4843-a7c2-d97620660673" />




### Demonstration 2 - Bad Multiplexer

Verilog Program:

```
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```


#### Synthesis Of Bad Multiplexer:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```



<img width="1280" height="762" alt="4" src="https://github.com/user-attachments/assets/09b7b6e6-dffb-4d27-b540-0e234132f0c3" />



#### Simulation Of Bad Multiplexer:

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```



<img width="1280" height="762" alt="5" src="https://github.com/user-attachments/assets/f4805a72-7a52-4e36-80c3-71d058ca7a16" />


#### GLS Of Bad Multiplexer:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```



<img width="1280" height="770" alt="6" src="https://github.com/user-attachments/assets/c422e576-2887-430a-88ec-861b0691ff89" />




### Demonstration 3 - Blocking Caveat

Verilog Program:

```
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```


#### Synthesis Of Blocking Caveat:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr blocking_caveat_net.v
show
```



<img width="1280" height="766" alt="7" src="https://github.com/user-attachments/assets/d7448b33-392e-4d0f-91d9-55b34b5ab08a" />


#### Simulation Of Blocking Caveat:

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```



<img width="1280" height="766" alt="8" src="https://github.com/user-attachments/assets/b0a679ba-9891-4fc4-ba0c-ab2f852142c9" />


#### GLS Of Blocking Caveat:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```



<img width="1280" height="758" alt="9" src="https://github.com/user-attachments/assets/2f373f83-ab07-499c-8ec4-2f7d434ee128" />






## Summary

Gate-level simulation (GLS) plays an important role in validating the synthesized netlist, ensuring that functionality, timing, and testability remain intact after synthesis. To avoid synthesis-simulation mismatches, it is crucial to stick to synthesizable RTL and write code that is both clear and unambiguous. A good coding practice is to use blocking (=) assignments when describing combinational logic, while reserving non-blocking (<=) assignments for sequential logic. Hands-on lab work further reinforces these concepts, helping you recognize common pitfalls in RTL design and strengthen your overall understanding.
