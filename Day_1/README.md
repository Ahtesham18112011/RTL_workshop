# Day 1: Introduction to Verilog RTL Design and Synthesis

Welcome to Day 1 of the RTL workshop! This session introduces the basics of Verilog HDL, an open-source simulator called Icarus Verilog (iverilog), and guides you through simulating Verilog code with testbenches and analyzing output waveforms. And also introduces the synthesizer Yosys.

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

This is a Verilog code for the multiplexer that is found in [`good_mux.v`](https://github.com/Ahtesham18112011/RTL_workshop/blob/main/Day_1/good_mux.v):

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

## What is Yosys?
Yosys is a free, open-source tool for digital hardware design. It takes code written in languages like Verilog or VHDL and turns it into a netlist—a blueprint of connected components like logic gates or flip-flops—that can be used to build circuits on chips like FPGAs or ASICs.

Here’s what Yosys does in simple terms:
- **Synthesis**: Converts your code into a basic circuit layout.
- **Optimization**: Makes the circuit smaller or faster by tweaking the design.
- **Technology Mapping**: Matches the circuit to specific hardware parts, like FPGA blocks or ASIC cells.
- **Verification**: Checks if the design works as intended with formal methods.
- **Extensibility**: Lets you add custom features or connect it with other tools.

## Why do we need  different flavors of gate in a `.lib` file?
A library file in digital design, often used with tools like Yosys, contains a collection of logic gates and cells (e.g., AND, OR, NOT, flip-flops) with varying characteristics.

Logic gates in a library file come in different "flavors" to give designers flexibility when synthesizing and optimizing digital circuits. Each flavor has unique characteristics, like size, speed, or power consumption, to meet specific design goals. Here's why we need them:

1. **Performance Optimization**: Some gates are faster (e.g., high-speed gates with lower delay) for critical paths where timing is crucial, while slower gates save power in less critical areas.

2. **Power Efficiency**: Low-power gates (e.g., with higher threshold voltages) reduce energy consumption, ideal for battery-powered devices, while high-power gates prioritize speed.

3. **Area Constraints**: Smaller gates save chip space, which is vital for compact designs, while larger gates might be used for better signal integrity or performance.

4. **Drive Strength**: Gates with different drive strengths (e.g., 1x, 2x, 4x) can drive varying numbers of connected components. Stronger gates handle higher loads but use more power and area.

5. **Signal Integrity**: Different gate flavors help manage issues like noise or signal degradation in complex circuits, ensuring reliable operation.

6. **Technology Mapping**: During synthesis (e.g., with Yosys), the tool selects the best gate flavor from the library to match the target hardware (FPGA or ASIC) and design constraints.

## Synthesis of the good_mux verilog code
Let's synthesis the above good_mux code using the open-source software Yosys.

Type the following command in the terminal:-
```shell
yosys
```
Then type the following command:-
```shell
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Then type the command for reading the verilog code:-
```shell
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
```
Then type the following command to synthesis the verilog code:-
```shell
synth -top good_mux
```
Then type:-
```shell
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Then finally to show the gate-level model for this verilog code type:-
```shell
show
```
![Screenshot_2025-05-28_20-38-22](https://github.com/user-attachments/assets/4b3a9939-92d0-4efc-ad69-e96faf19e6c3)

