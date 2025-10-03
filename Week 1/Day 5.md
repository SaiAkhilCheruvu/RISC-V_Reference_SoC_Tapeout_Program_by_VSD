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




## Labs For IF Statements

If statements are conditional constructs in Verilog that execute one block of code when a specified condition is true and an optional block when it is false.



### Incomplete If Statement (``` incomp_if ```):

```
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```



