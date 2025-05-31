# Day 4: GLS blocking vs non-blocking and Synthesis Simulation mismatch
Welcome to the day 4 of this workshop! This interesting day clears topics about GLS(Gate-Level Simulation), blocking and non-blocking statements in verilog and some Synthesis Simulation mismatch, which should be prevented to make a perfect netlist.

## What is GLS?
**GLS** stands for **Gate-Level Simulation**. It is a critical verification step in the VLSI design flow where the gate-level netlist of a digital circuit is simulated to validate its functionality, timing, and power characteristics.

### Key Points about GLS:
1. **Purpose**:
   - GLS verifies that the synthesized gate-level netlist (post-synthesis) accurately implements the intended functionality described in the RTL (Register Transfer Level) design.
   - It ensures the design behaves correctly after synthesis, accounting for gate delays, timing constraints, and technology-specific characteristics.

2. **When is GLS Performed?**:
   - After synthesis, where the RTL code is converted into a gate-level netlist (a representation of the design using actual gates from a standard cell library).
   - Before physical design (place-and-route) to catch issues early.

3. **What is Simulated?**:
   - The gate-level netlist, which includes logic gates, flip-flops, and interconnects.
   - Simulations include timing information (from SDF files, or Standard Delay Format) to model real-world delays and check timing behavior.

4. **Types of GLS**:
   - **Functional GLS**: Verifies the logical correctness of the netlist without considering timing (zero-delay or unit-delay models).
   - **Timing GLS**: Incorporates timing information to verify the design meets timing constraints under realistic conditions (e.g., setup/hold times, clock skew).

5. **Why is GLS Important?**:
   - **Synthesis Validation**: Ensures the synthesis tool correctly translated the RTL into gates.
   - **Timing Verification**: Detects timing violations (e.g., setup/hold time issues) not caught during static timing analysis (STA).
   - **Power Analysis**: Helps validate power estimates at the gate level.
   - **Testability**: Verifies the functionality of scan chains or test structures inserted for manufacturing testing (e.g., DFT - Design for Testability).


## What is Synthesis-Simulation Mismatch?

In the VLSI design flow:
- **Simulation** involves verifying the functionality of a circuit using its RTL (Register Transfer Level) description (written in languages like Verilog or VHDL) or a gate-level netlist.
- **Synthesis** is the process of converting the RTL code into a gate-level netlist, mapping it to a target technology library (e.g., standard cells or FPGA primitives).
- A **mismatch** occurs when the simulation results of the RTL (or pre-synthesis) differ from the simulation results of the synthesized netlist (post-synthesis) or the actual hardware behavior.

This mismatch can manifest as:
- Functional errors (e.g., incorrect logic outputs).
- Timing issues (e.g., setup/hold violations not caught in simulation).
- Power or performance discrepancies.

### Cause of Synthesis-Simulation Mismatch
**Improper RTL Coding Practices**:
   - **Non-synthesizable constructs**: Some RTL code may simulate correctly but include constructs (e.g., delays, initial blocks, or certain procedural assignments) that are not synthesizable or are interpreted differently by synthesis tools.
   - **Ambiguous or incomplete specifications**: Unclear coding, such as missing `else` clauses in case statements or improper sensitivity lists, can lead to different interpretations by simulators and synthesis tools.
   - Example: In Verilog, a combinational block with an incomplete sensitivity list may simulate correctly but synthesize into unintended sequential logic.




## Blocking vs Non-blocking statements in verilog 

### **Blocking Statements**
- **Syntax**: Uses the `=` operator.
- **Behavior**: 
  - Assignments are executed **sequentially** in the order they appear in the code.
  - The statement completes immediately, and the updated value is available for subsequent statements within the same time step.
  - They behave like typical assignments in procedural programming languages (e.g., C).
- **Use Case**: 
  - Typically used in **combinational logic** (e.g., `always @(*)` blocks) where the order of operations matters.
  - Suitable for temporary variables or intermediate calculations within a block.

### **Non-Blocking Statements**
- **Syntax**: Uses the `<=` operator.
- **Behavior**: 
  - Assignments are **scheduled** and executed **concurrently** at the end of the current time step (after all blocking assignments and other events in the same time step are processed).
  - The right-hand side of the assignment is evaluated immediately, but the left-hand side (the variable being assigned) is updated only after the scheduler processes all non-blocking assignments.
  - This mimics the parallel nature of hardware, where flip-flops update simultaneously on a clock edge.
- **Use Case**: 
  - Primarily used in **sequential logic** (e.g., `always @(posedge clk)` blocks) to model registers or flip-flops.
  - Ensures correct simulation of concurrent hardware updates.

| **Blocking (`=`)**                                 | **Non-Blocking (`<=`)**                            |
|---------------------------------------------------|---------------------------------------------------|
| Uses `=` operator                                 | Uses `<=` operator                                |
| Sequential, immediate execution                   | Concurrent, scheduled at end of time step         |
| Updates happen instantly in code order            | Updates applied after time step                   |
| For combinational logic, temporary variables       | For sequential logic, registers/flip-flops        |
| Infers combinational logic (e.g., gates)          | Infers sequential logic (e.g., flip-flops)        |
| Example: `always @(*) y = a & b;`                | Example: `always @(posedge clk) q <= d;`         |


## Labs
### Lab 1
The verilog code for the lab 1 is given below:-
```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
```


- **Module Name**: `ternary_operator_mux`
- **Inputs**:
  - `i0`: First input
  - `i1`: Second input
  - `sel`: Select line
- **Output**: `y`
- **Functionality**: The `assign` statement uses a ternary operator (`sel ? i1 : i0`) to select between `i1` and `i0` based on `sel`:
  - If `sel = 1`, `y = i1`
  - If `sel = 0`, `y = i0`

![lab1](https://github.com/user-attachments/assets/3f5eb05a-1861-4bb8-940c-6ff9f2af87fb)

### Lab 2
This lab do the synthesis of the  above lab's verilog code using Yosys. Follow the same steps to synthesisze
![lab2](https://github.com/user-attachments/assets/7a0cdc7c-cbbd-4943-bd3d-130a0d66b9b1)


### Lab 3
This lab is the GLS of the above verilog module the only change neede is using this command :-
```shell
iverilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```
![lab3](https://github.com/user-attachments/assets/9acf45b3-2e42-4ac1-88ae-b4a494cc8d87)


### Lab 4
The verilog code for the lab 4 is given below:-
```verilog
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

### Module Breakdown
- **Module Name**: `bad_mux`
- **Inputs**:
  - `i0`: First input
  - `i1`: Second input
  - `sel`: Select line
- **Output**: `y` (declared as `reg` due to the procedural assignment in the `always` block)
- **Functionality**: 
  - The `always @ (sel)` block triggers whenever `sel` changes.
  - If `sel = 1`, `y` is assigned `i1`.
  - If `sel = 0`, `y` is assigned `i0`.

### Issues with the Code
1. **Sensitivity List Issue**:
   - The `always` block only includes `sel` in the sensitivity list (`always @ (sel)`). In a multiplexer, the output `y` depends not only on `sel` but also on `i0` and `i1`. If `i0` or `i1` changes while `sel` remains constant, the output `y` won’t update, which is incorrect for a combinational circuit like a MUX.
   - **Fix**: Use `always @ (*)` (or `always_comb` in SystemVerilog) to include all inputs (`i0`, `i1`, `sel`) in the sensitivity list automatically.

2. **Non-Blocking Assignment in Combinational Logic**:
   - The module uses non-blocking assignments (`<=`) in the `always` block. Non-blocking assignments are typically used for sequential logic (e.g., flip-flops), not combinational logic like a MUX.
   - **Fix**: Use blocking assignments (`=`) for combinational logic to ensure immediate updates, e.g., `y = i1` and `y = i0`.


![lab4](https://github.com/user-attachments/assets/4c2ede06-0605-4ff0-99cb-fc89844b89e4)


### Lab 5
This lab is the GLS of the above `bad_mux.v` module.

![lab5](https://github.com/user-attachments/assets/2e698404-27b5-4c4a-a811-41b5fc13db77)


### Lab 6

The verilog code for the lab 4 is given below:-
```verilog
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```
The `blocking_caveat` module you provided is a Verilog module that implements combinational logic using an `always` block. However, the use of **blocking assignments** (`=`) in the `always` block introduces a potential issue, often referred to as a "blocking assignment caveat" in Verilog. Let’s analyze the module, identify the issue, and suggest improvements.

### Module Breakdown
- **Module Name**: `blocking_caveat`
- **Inputs**:
  - `a`: First input
  - `b`: Second input
  - `c`: Third input
- **Output**:
  - `d`: Output (declared as `reg` due to the procedural assignment)
- **Internal Signal**:
  - `x`: An internal `reg` used as an intermediate signal
- **Functionality**:
  - The `always @ (*)` block is combinational (sensitive to all inputs: `a`, `b`, `c`).
  - Inside the block:
    - `d = x & c`: Assigns the logical AND of `x` and `c` to `d`.
    - `x = a | b`: Assigns the logical OR of `a` and `b` to `x`.

![lab6](https://github.com/user-attachments/assets/42cac594-0008-4c7b-b415-43e6565b6081)


### Lab 7



















