# Day 3

Welcome to Day 3!
Today’s session explores optimization in digital design, focusing on both combinational and sequential circuits. You’ll learn foundational strategies to streamline logic, enhance speed, and reduce area and power—starting with optimizations for purely logic-based circuits, then moving to techniques relevant for sequential, state-retaining circuits.

## What is Optimization?

Optimization is the process of refining and squeezing the logic in a digital design to create a more efficient implementation. This means reducing the number of gates, lowering area, minimizing power consumption, or improving speed without changing the design’s functionality.

## Why Optimize?

- To save silicon area (reduce cost)

- To lower power usage (essential for battery-powered devices)

- To improve performance and meet timing requirements

## Combinational Optimization

Combinational optimization involves simplifying and minimizing logic in circuits without memory, focusing on reducing gate count, area, and power by optimizing Boolean expressions and logic structures.

**Direct Optimization:** Simplifies logic directly using algebraic methods and redundant logic removal.

*Example*: A + A·B = A

**Boolean Optimization:** Applies Boolean algebra rules and Karnaugh maps or algorithms to minimize logic expressions.

*Example*: A·B + A·̅B = A

## Sequential Optimization

Sequential optimization targets circuits with memory elements, aiming to reduce the number of states, flip-flops, and improve timing by optimizing state machines, retiming registers, and simplifying sequential logic paths.

### Basic Techniques:

- **Sequential Constant Propagation:** If a flip-flop always gets a constant value, it can be removed or simplified.

<img width="526" height="298" alt="image" src="https://github.com/user-attachments/assets/6de0eaf0-1f74-4365-afd7-2af68fcaf605" />

### Advanced Techniques:

- **State Optimization:** Merges equivalent or unused states in state machines to reduce flip-flops.

- **Retiming:** Moves flip-flops across logic gates to improve timing or reduce clock period.

<img width="792" height="446" alt="image" src="https://github.com/user-attachments/assets/7e14fbe3-bd47-4a87-bd59-43ec76d4e8cb" />

- **Logical Cloning:** Duplicates logic to balance timing paths or improve parallelism.

<img width="462" height="286" alt="image" src="https://github.com/user-attachments/assets/9de657a7-4757-4c48-b4bd-8dff6fc8c31b" />

## Demonstrating Combinational Optimisation

### Demonstration 1

**Verilog Program:**

To view the Verilog program, enter the following command in ``` verilog_files``` directory:

```
gvim opt_check.v
```


```
module opt_check (input a , input b , output y);
	 assign y = a?b:0;
endmodule
```

Analysis:


assign y = a ? b : 0;
```

- If a is true (1), then y is assigned the value of b.

- If a is false (0), then y is assigned 0.


```
Commands to optimise the module:

```
yosys                         # Start the Yosys synthesis shell
read_verilog opt_check.v      # Load the Verilog module into Yosys
synth -top opt_check          # Synthesize the top module 'opt_check'
opt_clean -purge              # Remove any unused or redundant logic signals
abc -liberty ../my-lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  # Map to standard cells using the library file
show                         # Visualize the optimized gate-level schematic
```

<img width="1280" height="800" alt="Screenshot from 2025-09-27 19-24-08" src="https://github.com/user-attachments/assets/3deca0f7-3a48-437d-92e1-8edab004c338" />

### Demonstration 2

**Verilog Program:**

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

Analysis:

```
assign y = a?1:b;
```

- If a is true (1), then y is assigned 1.
  
- If a is false (0), then y is assigned the value of b.

 <img width="1280" height="800" alt="Screenshot from 2025-09-27 19-27-48" src="https://github.com/user-attachments/assets/e84f6b3c-d23f-4cb0-bd54-0a72ab750261" />

 
### Demonstration 3

**Verilog Program:**

```
module opt_check2 (input a , input b , input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

Analysis:

```
assign y = a?(c?b:0):0;
```

- If a is true, check c.

    - If c is true, assign y the value of b.

    - If c is false, assign y the value 0.

- If a is false, assign y the value 0.

<img width="1280" height="800" alt="Screenshot from 2025-09-27 20-37-56" src="https://github.com/user-attachments/assets/0db7e090-492d-4f64-96b2-84ff9c47c0fc" />

### Demonstration 4

**Verilog Program:**

```
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
endmodule
```
Analysis:

```
assign y = a?(b?(a & c ):c):(!c);
```

- If a is true:

    - Then check b:

        - If b is true, assign y = a&c.

        - Otherwise, assign y = c.

- If a is false:

    - Assign y = NOT c.


<img width="1280" height="800" alt="Screenshot from 2025-09-27 20-40-48" src="https://github.com/user-attachments/assets/0523a6ce-65a4-4b25-b9ab-13ade2164c70" />


## Demonstrating Sequential Optimisation

### Demonstration 1

**Verilog Program:**

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

This code defines a D flip-flop with an asynchronous reset input. When the reset signal is activated (high), the output qq is immediately set to 0 regardless of the clock. When reset is inactive, on every positive clock edge, qq is set to a constant value 1.


<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-09-01" src="https://github.com/user-attachments/assets/9c380f5a-260b-4cb0-ba08-249f8b0fd42a" />

<img width="1280" height="800" alt="Screenshot from 2025-09-27 20-57-08" src="https://github.com/user-attachments/assets/0430ba8a-5f61-42a8-8f1d-c82178ce9787" />


### Demonstration 2

**Verilog Program:**

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

The output q is always set to 1 by the D flip flip irrespective of reset or clock

<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-12-16" src="https://github.com/user-attachments/assets/f9bfde08-629a-4f21-aa74-b371da917dd2" />

<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-02-55" src="https://github.com/user-attachments/assets/6b2bf581-85cd-4b41-9c65-427f607de55d" />


### Demonstration 3

**Verilog Program:**

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
  if(reset) begin
    q <= 1'b1;    // On reset, immediately set q to 1
    q1 <= 1'b0;   // Set intermediate register q1 to 0
  end else begin
    q1 <= 1'b1;   // On clock edge, q1 set to 1
    q <= q1;      // q takes previous value of q1 (0) for one cycle, then remains 1
  end
end
endmodule
```

Provides asynchronous reset that sets output q to 1 instantly, but output briefly goes to 0 for one clock cycle after reset is released, then stays at 1.


<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-18-37" src="https://github.com/user-attachments/assets/5ad4da64-f7a7-4dd0-a93c-5959ad387517" />


<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-16-05" src="https://github.com/user-attachments/assets/83373652-2240-4c15-9366-649d9f7cd67d" />


## Sequential Optimisation For Unused Outputs

### Program 1

```
module counter_opt (input clk, input reset, output q);
  reg [2:0] count;
  assign q = count[0];

  always @(posedge clk, posedge reset)
  begin
    if(reset)
      count <= 3'b000;
    else
      count <= count + 1;
  end
endmodule
```
<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-22-07" src="https://github.com/user-attachments/assets/c4f85f15-477a-4fba-a7b5-97937025ecff" />

### Program 2

```
// Code 2: counter_opt2.v
module counter_opt (input clk, input reset, output q);
  reg [2:0] count;
  assign q = (count[2:0] == 3'b100);

  always @(posedge clk, posedge reset)
  begin
    if(reset)
      count <= 3'b000;
    else
      count <= count + 1;
  end
endmodule
```

<img width="1280" height="800" alt="Screenshot from 2025-09-27 21-27-05" src="https://github.com/user-attachments/assets/e6cddf31-31ff-4485-a8ca-5cde8a37afa2" />



Both programs use a 3-bit register implemented with 3 D flip-flops, but Program 1 outputs the least significant bit of the counter directly as the output signal, while Program 2 outputs a high signal only when the entire 3-bit count exactly equals 4, acting as a comparator flag.


## Summary:

Day 3 focused on optimization methods used in digital circuit design targeting both combinational and sequential logic. Key techniques covered include:

- **Constant Propagation:** Simplifying circuits by substituting constant values to reduce complexity.

- **State Optimization:** Minimizing and efficiently encoding states in state machines for power and area savings.

- **Cloning:** Duplicating logic modules to balance timing and improve performance.

- **Retiming:** Shifting registers within the circuit to optimize speed without changing circuit behavior.

Verilog labs demonstrating these concepts were performed, with examples of logic optimization and D flip-flop behaviors, complete with code snippets and output illustrations.



