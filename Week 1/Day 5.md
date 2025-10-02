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



