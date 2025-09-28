# Day 4

Day 4 focused on understanding the importance of gate-level simulation (GLS), the differences between blocking and non-blocking statements in Verilog, and how mistakes in RTL coding styles can result in simulation-synthesis mismatches.


## What Is GLS?

Gate Level Simulation (GLS) is running a testbench on the gate-level netlist (post-synthesis representation). The netlist is a logical equivalent of the RTL, so the same testbench can be used to verify the design at this stage.

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

verilog
always @(*) begin
  y = a & b;      // y gets the value of a AND b immediately
  z = y | c;      // z uses the updated value of y
end

Another example in a clocked block:

verilog
always @(posedge clk) begin
  b = a;
  c = b;
end

Here, c will get the value of a right away, because each assignment happens in order.
Non-Blocking Assignment Example (<=)

Non-blocking assignments schedule updates to happen at the end of the time step, allowing parallel updates. This is best for sequential logic.

verilog
always @(posedge clk) begin
  q1 <= d;
  q2 <= q1;
  q3 <= q2;
end

In this example, the value of d will propagate to q1, then to q2, then to q3 over three clock cycles, modeling a shift register.
  

