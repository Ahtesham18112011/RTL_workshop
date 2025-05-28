# Day 1: Introduction to Verilog RTL Design and Synthesis

Welcome to Day 1 of the RTL workshop! This session introduces the basics of Verilog HDL, an open-source simulator called Icarus Verilog (iverilog), and guides you through simulating Verilog code with testbenches and analyzing output waveforms.

---

## Prerequisites

- **Linux environment** (Ubuntu/Debian recommended)
- **Basic terminal knowledge**

---

## Contents

1. [Introduction to Open-Source Simulator: iverilog](#1-introduction-to-open-source-simulator-iverilog)
2. [Lab: Simulating a 2-to-1 Multiplexer (good_mux) using iverilog](#2-lab-good_mux-using-iverilog)
3. [Code Analysis](#analysis-of-the-verilog-code)
4. [Summary](#summary)

---

## 1. Introduction to Open-Source Simulator: iverilog

### What is a Simulator?

A simulator is a software tool used to test the functionality of a digital circuit design by applying different input stimuli and observing the output. It allows you to validate your design before hardware fabrication.

### What is a Design?

A design is the actual Verilog code (or set of codes) that implements the required digital logic.

### What is a Testbench?

A Verilog testbench is a simulation environment used to verify the functionality and correctness of a digital design described in Verilog HDL. It applies different inputs and scenarios to the design, allowing you to check its behavior before moving to physical hardware.

![Screenshot (183)](https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757)

### Iverilog Simulation Flow

![Screenshot (184)](https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba)

As shown above, both the design and testbench are provided as input to iverilog. The simulator generates a `.vcd` file, which can be viewed as waveforms using GTKWave.

---

## 2. Lab: good_mux Using iverilog

Let's simulate a 2-to-1 multiplexer using iverilog. Follow these steps:

### Step 1: Clone the Required Repository

```shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### Step 2: Install Required Tools

Install iverilog (simulator) and gtkwave (waveform viewer):

```shell
sudo apt install iverilog
sudo apt install gtkwave
```

### Step 3: Simulate the Design

Compile the design and testbench:

```shell
iverilog good_mux.v tb_good_mux.v
```

Run the simulation:

```shell
./a.out
```

Generate and view the waveform:

```shell
gtkwave tb_good_mux.vcd
```

GTKWave will open, allowing you to observe the simulated waveforms.

![Screenshot_2025-05-28_12-39-30](https://github.com/user-attachments/assets/701e8189-3101-4a82-8134-e799521b9a8b)

---

## Analysis of the Verilog Code

The Verilog code for the multiplexer is found in [`good_mux.v`](https://github.com/Ahtesham18112011/RTL_workshop/blob/main/Day_1/good_mux.v):

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

**Explanation:**

- **Module Declaration:**
  - Inputs: `i0`, `i1` (data inputs), `sel` (select line)
  - Output: `y` (registered output)

- **Functionality:**
  - The `always @ (*)` block describes combinational logic, sensitive to all inputs.
  - If `sel` is `1`, the output `y` is assigned the value of `i1`.
  - If `sel` is `0`, the output `y` is assigned the value of `i0`.

---

## Summary

On Day 1, you learned how to:

- Use iverilog to simulate Verilog designs and testbenches
- Install and use GTKWave to analyze simulation results
- Understand the role of testbenches in digital design verification
- Simulate a simple 2-to-1 multiplexer and interpret its output


---






