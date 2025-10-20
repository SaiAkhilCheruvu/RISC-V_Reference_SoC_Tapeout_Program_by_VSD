# Day 5

This session covers Verilog synthesis optimization, including efficient if-else statements, for-loops, and generate blocks, while highlighting how improper coding (like missing else branches) can cause inferred latches. Labs were performed to apply the concepts and examine the resulting synthesized designs.


## If-Else Statements

- Used to make conditional decisions in Verilog.

- Executes one block when a condition is true, another block when false.
  

```
always @(*) begin
    if (a > b)
        y = a;
    else
        y = b;
end
```



## Nested If-Else Statements

- An if-else inside another if-else, used for multiple conditions.

- Checks conditions sequentially.


```
always @(*) begin
    if (a > b) begin
        y = a;
    end else if (b > c) begin
        y = b;
    end else begin
        y = c;
    end
end
```


## Latch Inference in Verilog

- Occurs when a variable in a combinational block is not assigned in all possible conditions.

- Usually unintentional and can cause unpredictable hardware behavior.

- Can be avoided by assigning default values or using complete if-else or case statements.


```

module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        if (sel == 1'b1)
            y = a; // No 'else' - y is not assigned when sel == 0
    end
endmodule

```

**Problem:** When ```sel``` is 0, ```y``` is not assigned, so a latch is inferred.

**Solution:** Add an ```else``` or ```default``` case:

```

module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        case(sel)
            1'b1 : y = a;
            default : y = 1'b0; // Default assignment
        endcase
    end
endmodule
```

## Labs For Simulation And Synthesis

Navigate to the ```verilog_files``` directory

### Steps For Simulation:

1. Compile the Design and Testbench:

   ```
   iverilog design.v tb_design.v
   ```

2. Execute the generated simulation binary to produce a VCD waveform file:

   ```
   ./a.out
   ```

3. Open the VCD file with GTKWave to view signal transitions:

   ```
   gtkwave tb_design.vcd
   ```


### Steps For Synthesis:

1. Start Yosys:

   ```
   yosys
   ```

2. Inside Yosys, read the standard cell library:

   ```
   read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

3. Read your Verilog design file:

   ```
   read_verilog design.v
   ```

4. Specify the top model to synthesize:

   ```
   synth -top design
   ```

5. Run technology mapping with your liberty file:

   ```
   abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. Visualize the synthesized logic:

   ```
   show
   ```


## Syntax Examples

**If Statements:**

```
always @(*) begin
  if (condition) begin
    // code executed if condition is true
  end
end
```

**If Else Statements:**

```
always @(*) begin
  if (condition) begin
    // code if condition is true
  end else begin
    // code if condition is false
  end
end
```

**Case Statements:**

```
always @(*) begin
    case (expression)
        value1: begin
            // statements executed when expression == value1
        end
        value2: begin
            // statements executed when expression == value2
        end
        default: begin
            // statements executed if no match found
        end
    endcase
end
```


**For Loops:**

```
integer i;
always @(*) begin
    for (i = 0; i < N; i = i + 1) begin
        // code repeated N times
    end
end
```


## Labs For IF Statements

If statements are conditional constructs in Verilog that execute one block of code when a specified condition is true and an optional block when it is false.



### Incomplete If Statement (```incomp_if ```):

```
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```

This Verilog code defines a module where the output y is assigned the value of input i1 only when input i0 is true. The assignment happens inside an always @(*) block, which means it reacts to any changes in the inputs. Since there is no else condition, y keeps its previous value when i0 is false, potentially causing unintended latch behavior. Essentially, the module conditionally updates y based on i0 and i1 in combinational logic.


#### Simulation Results:


<img width="1280" height="758" alt="1" src="https://github.com/user-attachments/assets/c7dc5c2d-2e4c-41a6-ab2f-45d75e03d1d0" />




#### Synthesis Results:


<img width="1280" height="750" alt="2" src="https://github.com/user-attachments/assets/dfb62cec-d471-4c50-bc4e-87fbb98bd9ac" />



### Nested If Else Statement (```incomp_if2 ```):

```
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

This Verilog module uses an always @(*) block to model combinational logic with multiple conditions. Inside the block, if input i0 is true, the output y is assigned i1. If i0 is false but i2 is true, then y is assigned i3. If neither condition is true, y remains unchanged. This conditional structure ensures y updates based on the priority of conditions, avoiding latch inference because all conditions that affect y are covered in the if-else-if chain. The non-blocking assignments (<=) are used for output updates inside the always block.


#### Simulation Results:


<img width="1280" height="754" alt="3" src="https://github.com/user-attachments/assets/5d4c9178-ea49-4f13-b584-016605c65572" />




#### Synthesis Results:



<img width="1280" height="770" alt="4" src="https://github.com/user-attachments/assets/ab6f0db2-8049-4d7e-a112-1ceb94aa2dff" />



## Labs For Case Statements

Case statements are conditional constructs in Verilog that select and execute one block of code from multiple possibilities based on the value of a signal, providing a clear way to handle multiple discrete conditions.


### Incomplete Case (```incomp_case```):

```
module incomp_case  (input i0, input i1, input i2, input [1:0], output reg y);
always @ (*)
begin 
case(sel)
2'b00 : y = i0;
2'b01 : y = i1;
endcase
end
endmodule
```

This Verilog module "incomp_case" uses a case statement inside an always @(*) block to assign the output y based on the value of a 2-bit selector input sel. When sel is 2'b00, y is assigned the value of input i0; when sel is 2'b01, y is assigned the value of i1. The case statement allows selection among multiple input conditions in a clear and structured way, similar to a switch statement in other languages. Since only specific cases are covered and no default is provided, other values of sel will result in y retaining its previous value or inferred latch behavior.


#### Simulation Results:

<img width="1280" height="758" alt="5" src="https://github.com/user-attachments/assets/62e43c99-6c88-44c2-bd0b-1f7561681048" />




#### Synthesis Results:

<img width="1280" height="770" alt="6" src="https://github.com/user-attachments/assets/cde2c7fe-48e5-417b-98d5-2ed6b6afe76c" />




### Complete Case (```comp_case```):

```
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```

The code uses a case statement inside an always block to select the output based on the 2-bit selector value. For selector values 2'b00 and 2'b01, it assigns corresponding inputs to the output. For all other selector values, the output is set to a default input. Including the default case ensures all possible selector values are covered, preventing unintended latch behavior and making the logic more robust.


#### Simulation Results:


<img width="1280" height="758" alt="7" src="https://github.com/user-attachments/assets/6dda089c-3bb8-44ea-9002-27e122f7796b" />



#### Synthesis Results:


<img width="1280" height="766" alt="8" src="https://github.com/user-attachments/assets/fc7dd742-759f-4ad1-99d7-a4b3cb1844d6" />




### Bad Case (```bad_case```):

```
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // '?' is a wildcard; be careful with incomplete cases!
    endcase
end
endmodule
```

The code uses a case statement with a wildcard pattern (?) in one case item, allowing it to match multiple values of the selector. This wildcard represents a "don't care" bit, which matches either 0 or 1 in that bit position. However, using wildcards can be risky because if cases overlap, the first matching case takes priority, which might lead to unexpected behavior. This kind of case statement is useful for concise priority logic but needs careful handling to avoid logic conflicts.


#### Simulation Results:


<img width="1280" height="758" alt="9" src="https://github.com/user-attachments/assets/a5195458-47ab-444e-947d-6dc8ad1a7fb0" />




#### Synthesis Results:


<img width="1280" height="766" alt="10" src="https://github.com/user-attachments/assets/6eb55ca3-fd24-46b1-8d4b-5138f3093cdd" />




### Partial Assignments in Case (```partial_case_assign```):

```
module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1;
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```

The code uses a case statement inside an always block to assign values to two outputs based on the selector input. For sel = 2'b00, both outputs are assigned specific input values. For sel = 2'b01, only one output is assigned, while the other keeps its previous value, which can lead to unintended latch behavior. The default case assigns both outputs to inputs, covering all other selector values and helping to avoid incomplete logic in synthesis. This structure models multiplexing with multiple outputs, but partial assignments in some cases should be handled carefully to avoid latches.



#### Synthesis Results:


<img width="1280" height="770" alt="11" src="https://github.com/user-attachments/assets/79528b6a-502f-4707-9330-b52907b565ab" />



## Labs For Loops and Generate Blocks


For Loops are iterative constructs in Verilog used to repeat a block of code a specific number of times, commonly within always blocks for behavioral modeling or testbenches.

Generate Blocks are compile-time constructs used to replicate hardware structures or instantiate multiple modules systematically, often combined with for loops for scalable design.


### 4-to-1 MUX Using For Loop (```mux_generate```):

```
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```

This code implements a 4-to-1 multiplexer using a generate-like approach with a for-loop inside an always block. It concatenates the four inputs into a 4-bit wire vector, then iterates through each bit index. When the loop index matches the selector sel, it assigns the corresponding bit of the input vector to the output y. This method dynamically selects one input based on sel by scanning all inputs in a loop.


#### Simulation Results:


<img width="1280" height="762" alt="12" src="https://github.com/user-attachments/assets/ab2961d0-1278-46dc-a7cf-5a1ab160d046" />




#### Synthesis Results:


<img width="1280" height="770" alt="13" src="https://github.com/user-attachments/assets/4de290b0-076c-472a-8d2c-c0016ba4d997" />




### 8-to-1 Demux Using Case (```demux_case```):

```
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```

This code implements an 8-output demultiplexer. The 3-bit selector sel chooses which one of the eight outputs gets the input i signal. Inside the always block, an 8-bit register is initially cleared, then based on sel, the corresponding bit in the register is set to i. The register bits are connected directly to the outputs, so only the selected output reflects the input signal while others remain zero.



#### Simulation Results:


<img width="1280" height="770" alt="14" src="https://github.com/user-attachments/assets/68760334-b364-4187-9675-73bbdae449b9" />






#### Synthesis Results:


<img width="1280" height="770" alt="15" src="https://github.com/user-attachments/assets/80f4832e-b9f1-48e7-9cdf-72c22d2584b4" />





### 8-to-1 Demux Using For Loop (```demux_generate```):

```
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```

This code implements an 8-output demultiplexer using a for-loop inside an always block. It clears an 8-bit register initially, then iterates through each bit index. When the loop index matches the selector sel, it assigns the input i to the corresponding bit of the register. The register bits are connected directly to the outputs, ensuring This code describes a full adder using continuous assignment. It adds three single-bit inputs (a, b, c) as binary values, producing a 2-bit result where the most significant bit is the carry-out (co) and the least significant bit is the sum (sum). This succinctly models the full adder functionality in combinational logic.


#### Simulation Results:

<img width="1280" height="758" alt="16" src="https://github.com/user-attachments/assets/ab53d28d-4407-4a47-b1c0-5f0d03412c90" />






#### Synthesis Results:


<img width="1280" height="762" alt="17" src="https://github.com/user-attachments/assets/df2efcc7-e903-4489-9ed2-4e4341abf9f3" />






### Full Adder (```fa```):

```
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
This code describes a full adder using continuous assignment. It adds three single-bit inputs (a, b, c) as binary values, producing a 2-bit result where the most significant bit is the carry-out (co) and the least significant bit is the sum (sum). This succinctly models the full adder functionality in combinational logic.



#### Synthesis Results:


<img width="1280" height="766" alt="18" src="https://github.com/user-attachments/assets/3cc56b32-72af-4a6d-8877-b735445ee45a" />



### What Is A Ripple Carry Adder (RCA)?


A ripple carry adder is a digital circuit that adds two binary numbers by connecting multiple full adders in series. The carry-out from each full adder "ripples" to the next adder as carry-in. While simple and modular, the carry propagation through all stages causes delay, making it slower for larger bit-widths. It forms a fundamental structure for binary addition in digital systems.


<img width="781" height="329" alt="image" src="https://github.com/user-attachments/assets/484b7315-3807-4689-936c-9e33afa12c8c" />


### 8-bit Ripple Carry Adder with Generate Block (```rca```):

```
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

This code implements an 8-bit ripple carry adder using full adders. It adds two 8-bit inputs (num1 and num2) and produces a 9-bit output sum to accommodate the carry out. The least significant bit is added using a full adder with carry-in zero. Subsequent bits are added using a generate-for loop, where each full adder takes carry-in from the previous bit's carry-out. The final carry-out forms the most significant bit of the sum, completing the ripple carry addition.


#### Simulation Results:

<img width="1280" height="758" alt="19" src="https://github.com/user-attachments/assets/118deb80-8c37-4f49-a689-d62434bc07f9" />





#### Synthesis Results:


<img width="1280" height="766" alt="20" src="https://github.com/user-attachments/assets/be995673-c941-4d33-bcea-44eff057a5b1" />




## Summary 


Full if-else and case constructs were employed to prevent unwanted latch generation. For loops and generate blocks were utilized to create scalable and synthesis-friendly designs. All signals were ensured to be assigned in every possible execution path when coding combinational logic. Practical lab exercises helped solidify these principles through hands-on Verilog coding and synthesis verification.
