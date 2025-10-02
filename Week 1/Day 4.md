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


#### Synthesis Of Ternary Operator Multiplexer

<img width="1280" height="800" alt="Screenshot from 2025-09-28 17-53-12" src="https://github.com/user-attachments/assets/98cd9a75-885b-481d-bfa1-92d1254402b7" />


#### Simulation Of Ternary Operator Multiplexer

<img width="1280" height="800" alt="Screenshot from 2025-09-28 17-48-50" src="https://github.com/user-attachments/assets/b711dcb7-0b1c-4b75-8460-1497e9ba4951" />


#### GLS Of Ternary Operator Multiplexer

Use the following command, modifying the file paths as needed, to run a gate-level simulation (GLS):

```
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```




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

<img width="1280" height="800" alt="Screenshot from 2025-09-28 18-21-46" src="https://github.com/user-attachments/assets/d1fa143c-a05a-466e-8b26-5fdcd2906f58" />

<img width="1280" height="800" alt="Screenshot from 2025-09-28 18-42-22" src="https://github.com/user-attachments/assets/f20fe3bc-c50a-4253-8946-a1c41055b951" />




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

<img width="1280" height="800" alt="Screenshot from 2025-09-28 18-22-25" src="https://github.com/user-attachments/assets/a314052c-1b9a-4da9-980c-47ed58973979" />

<img width="1280" height="800" alt="Screenshot from 2025-09-28 18-43-13" src="https://github.com/user-attachments/assets/2f62df62-8ded-42ee-9b08-ac092090e8ab" />

