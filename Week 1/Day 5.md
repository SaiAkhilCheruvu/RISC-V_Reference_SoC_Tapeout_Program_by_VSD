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





