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
