
# Day 1

Welcome to Day 1 of the RTL Workshop. In this session, we focused on understanding the fundamentals of Verilog RTL design and synthesis. We started by working on a 2:1 multiplexer module, simulating its behavior using iverilog, and analyzing the signal waveforms in GTKWave to verify functionality. Next, we explored the synthesis process using Yosys, generating gate-level netlists compatible with the Sky130 PDK.

This hands-on experience laid a strong foundation in digital design workflows, from writing RTL code and running simulations to synthesizing designs using open-source tools.

Let's now proceed to review the detailed contents covered during this session.

## What is Design?

A design refers to the Verilog code or a collection of Verilog codes created to perform a specific intended functionality according to required specifications.

## What is a Testbench?

A testbench is a setup that applies stimulus (test vectors) to the design in order to verify and check its functionality.

image

This diagram shows the testbench setup with stimulus generation, design under test, and output observation. The design has primary inputs and outputs, while the testbench itself does not.

## What is a Simulator?

A simulator is a tool used to check if the RTL design adheres to the specification by simulating the behavior of the design.
