# Day 4: GLS blocking vs non-blocking and Synthesis Simulation mismatch
Welcome to the day 4 of this workshop! This interesting day clears topics about GLS(Gate-Level Simulation), blocking and non-blocking statements in verilog and some Synthesis Simulation mismatch, which should be prevented to make a perfect netlist.

## What is GLS?
In VLSI (Very Large Scale Integration), **GLS** stands for **Gate-Level Simulation**. It is a critical verification step in the VLSI design flow where the gate-level netlist of a digital circuit is simulated to validate its functionality, timing, and power characteristics.

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

