# Day 3: Combinational and Sequential optimization
Welcome to the day 3 of this workshop! this day discusses about optimization of combinational and sequential circuits. And introduces us to to the techniques for optimization.

## constant propagation: a technique for optimization

In VLSI design, constant propagation is a compiler optimization technique used to replace variables with their constant values during the synthesis process. This optimization can simplify the design, potentially reducing circuit complexity and improving performance by eliminating unnecessary logic. 

**How it works**:

Constant propagation involves analyzing the design description (e.g., a hardware description language like Verilog or VHDL) and identifying variables that have constant values. These variables are then replaced with their actual constant values in subsequent operations. 

**Benefits**:

* Reduced Complexity: By replacing variables with constants, the synthesis tool can potentially simplify the logic, leading to a smaller circuit. 
* Performance Improvement: Simplified logic can result in faster execution and reduced propagation delays. 
* Resource Optimization: Constant propagation can help optimize resource utilization, potentially reducing the number of logic gates or flip-flops required. 






