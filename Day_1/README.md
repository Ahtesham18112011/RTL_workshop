# Day 1: Introduction to Verilog RTL Design and Synthesis
This Day of the workshop introduces us to the Verilog HDL and explains a Verilog simulator known as Iverilog and its workings. Then explains us how to simulate our Verilog codes with Testbenches and how to obtain a `.vcd` file from the testbench. And then, introduces  us to the Yosys synthesiser, and finally about Sky130 PDK.

**This day is divide into the following parts:**

## 1. Introduction to open-source simulator: iverilog

### Simulator
A simulator is a software tool used to test the functionality of a digital circuit design by applying different input stimuli and observing the output behavior. It depends on the inputs of the design, and changes according to the inputs.

### Design
Design is the actual verilog code or set of verilog codes which has the intended functionality to meet with the required specifications.

### Testbench
Verilog testbench is a simulation environment used to verify the functionality and correctness of a digital design described in the Verilog hardware description language (HDL).
The purpose of a testbench is to provide a way to simulate the behavior of the design under various conditions, inputs, and scenarios before actually fabricating the physical hardware. It allows designers to catch bugs, validate functionality, and optimize designs without the cost and time associated with physical prototyping.

![Screenshot (183)](https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757)

### Iverilog simulation flow

![Screenshot (184)](https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba)

As you can see, we put the design and the testbench to iverilog and, in return, it gives us the `.vcd` file which can be used to generate waveforms.

## 2. Lab: good-mux using iverilog

So, first we need to clone the repository which has all the necessory verilog filer and testbenches. Type the following command in the Linux terminal:-

```shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```



