# Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding styles
Welcome to the day 2 of this workshop! This day of the  workshop first explains  the `.lib` file named as `sky130_fd_sc_hd__tt_025C_1v80.lib` which is an open-source PDK for making Integrated Circuits.
Then explains the difference between hierarchical and flat synthesis and finally tells about the various styles of Flip-Flop coding.

## Timing libs
### SKY130 PDK

The SKY130 PDK is an open-source Process Design Kit (PDK) based on SkyWater Technology's 130nm CMOS technology. It's a crucial resource for designing integrated circuits (ICs) using this specific technology node. If you have cloned the repository discussed in day 1 you must have the Sky130 PDK liberty downloaded named as `sky130_fd_sc_hd__tt_025C_1v80.lib`. 

### What is the meaning and use of tt_025C_1v80 in the sky130 PDK?
You would have this question in your mind when you saw the file name. Actually the tt stands for typical process, and 025C is the temperarure in the chip because CMOS technique is too sensitiive on temperatures, and the 1v80 is the voltage.


---

### Opening the `lib` file

To open the `sky130_fd_sc_hd__tt_025C_1v80.lib` file we need to type the following commands in the terminal:-

First, we need to type this command for downloading the `.lib` file veiwer.
```shell
sudo apt install gedit
```
Second, after installation type the command after navigating to the directory where your `.lib` file is present to open the `.lib` file:-
```shell
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/user-attachments/assets/8ab83ac1-d95d-495a-995b-b98944c6fe28)


---

## Hierarchical vs Flattened Synthesis
### **Hierarchical Synthesis**
- **Definition**: Hierarchical synthesis preserves the module hierarchy of the design as defined in the original RTL (Register-Transfer Level) code. Each module is synthesized separately, and the boundaries between modules are maintained.
- **How it Works**:
  - Yosys processes each module in the design hierarchy independently, performing optimizations.
  - The `hierarchy` command in Yosys is typically used early in the synthesis flow to analyze and set up the design hierarchy, ensuring the top module is identified and unused modules are removed.
  
- **Advantages**:
  - **Faster Runtime**: Since optimizations are applied module by module, hierarchical synthesis can be faster, especially for large designs with many modules, as it avoids processing the entire design as a single unit.
  - **Better Debugging and Analysis**: Preserving the hierarchy makes it easier to map the synthesized netlist back to the original RTL, aiding in debugging, timing analysis, and area/power reporting for specific modules.
  - **Modularity**: Useful for designs where maintaining module boundaries is important, such as when integrating with other tools (e.g., place-and-route) that rely on hierarchical information.
- **Disadvantages**:
  - **Limited Optimization**: Optimizations are restricted to within each module, so opportunities for cross-module optimizations (e.g., sharing logic across modules) are missed, potentially leading to a less optimized design in terms of area or performance.
  - **Complex Reporting**: Generating detailed reports (e.g., area, power) for each hierarchical entity requires additional steps, such as using the `stat` command with a liberty file.

### Example
Below is the example of a synthesized hierarchical verilog  module:-


![Screenshot_2025-05-29_19-04-48](https://github.com/user-attachments/assets/ffb36278-3d7d-457a-8236-c63e3ad2fd36)



---

### **Flattened Synthesis**
- **Definition**: Flattened synthesis collapses the entire design hierarchy into a single, flat module, removing all module boundaries. The design is treated as one large netlist during synthesis.
- **How it Works**:
  - The `flatten` command in Yosys replaces all module instances with their implementations, effectively merging all submodules into the top-level module.
  - This results in a single module containing all the logic, which is then optimized as a whole.

- **Advantages**:
  - **Maximum Optimization**: By treating the design as a single unit, Yosys can perform aggressive optimizations across the entire design, such as logic sharing, constant propagation, and gate reduction, potentially leading to a smaller or faster netlist.
  - **Simpler Netlist**: A flattened design results in a single module, which can simplify certain downstream processes, such as logic equivalence checking or some forms of place-and-route.
  - **Better Performance**: Cross-module optimizations can improve timing or reduce area compared to hierarchical synthesis.
- **Disadvantages**:
  - **Longer Runtime**: Flattening can significantly increase synthesis time for large designs, as the entire design is processed as a single unit.
  - **Loss of Hierarchy**: The flattened netlist loses the original module structure, which can make debugging, timing analysis, and area/power reporting more challenging, as it’s harder to trace back to the RTL.
  - **Increased Complexity**: For very large designs, a flattened netlist can become unwieldy, increasing memory usage and complicating physical implementation.
### Example
Below is an example of the same verilog as of above code but flattened. This can be done just by writing the command flatten in the terminal after the normal synthesis.

![Screenshot_2025-05-29_19-20-47](https://github.com/user-attachments/assets/3bb17602-62f1-4f6b-94ea-c279ac04754f)

> [!IMPORTANT]
> It is important to notice the difference between a hierarchical model and a flatten model. In hierarchical, the design was build with the sub-modules, in the flatten the design was build from breakdown of each sub-modules.





### **Key Differences**
| Aspect                  | Hierarchical Synthesis                     | Flattened Synthesis                       |
|-------------------------|--------------------------------------------|------------------------------------------|
| **Hierarchy**           | Preserves module boundaries                | Collapses all modules into one           |
| **Optimization Scope**  | Within each module                         | Across the entire design                 |
| **Runtime**             | Faster for large designs                   | Slower, especially for large designs     |
| **Debugging**           | Easier to trace back to RTL                | Harder due to loss of hierarchy          |
| **Output Complexity**   | Maintains modular structure                | Single, potentially complex netlist      |
| **Use Case**            | Modularity, debugging, reporting           | Maximum optimization, simpler netlist    |





---

## Synthesis with Flip-Flops
A flip flop is an electronic circuit with two stable states that can be used to store binary data. The stored data can be changed by applying varying inputs. Flip-flops and latches are fundamental building blocks of digital electronics systems used in computers, communications, and many other types of systems. It is used to recover the glitch caused by delays of the gates in combinational circuits also.

### Analysis of the verilog code for asynchronous reset D Flip-Flop
Below is the verilog module for a asynchronous reset D Flip-Flop.
```verilog
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```


### Module Analysis
- **Module Name**: `dff_asyncres`
- **Inputs**:
  - `clk`: Clock signal (triggers on positive edge).
  - `async_reset`: Asynchronous reset signal (active high, triggers on positive edge).
  - `d`: Data input to the flip-flop.
- **Output**:
  - `q`: Registered output (single bit, declared as `reg`).
- **Functionality**:
  - The flip-flop captures the value of `d` on the rising edge of `clk` and assigns it to `q`.
  - If `async_reset` is high (on its rising edge), `q` is reset to `1'b0` (0) regardless of the clock.
  - The reset is asynchronous, meaning it does not depend on the clock signal.

### Code Explanation
```verilog
module dff_asyncres ( input clk, input async_reset, input d, output reg q );
```
- Declares the module with three inputs (`clk`, `async_reset`, `d`) and one output (`q`).

```verilog
always @ (posedge clk, posedge async_reset)
```
- The `always` block is sensitive to:
  - The positive (rising) edge of `clk`.
  - The positive (rising) edge of `async_reset`.

```verilog
begin
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;
end
```
- If `async_reset` is high (1), `q` is set to `1'b0` (0).
- Otherwise, on the rising edge of `clk`, `q` takes the value of `d`.
- The `<=` operator indicates non-blocking assignments, which are standard for sequential logic in Verilog.

### Key Characteristics
- **Asynchronous Reset**: The reset (`async_reset`) takes precedence over the clock and can change `q` immediately when `async_reset` goes high, without waiting for a clock edge.
- **Edge-Triggered**: The data (`d`) is captured and assigned to `q` only on the rising edge of `clk` when `async_reset` is low.
- **Single-Bit Storage**: The flip-flop stores a single bit, as `q` and `d` are single-bit signals.


### Analysis of the verilog code for asynchronous set D Flip-Flop
Below is the verilog code for a asynchronous set D Flip-Flop:-
```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```
The provided Verilog code describes a D flip-flop (DFF) with an asynchronous set. Let me analyze the module and explain its functionality:

### Module Analysis
- **Module Name**: `dff_async_set`
- **Inputs**:
  - `clk`: Clock signal (triggers on positive edge).
  - `async_set`: Asynchronous set signal (active high, triggers on positive edge).
  - `d`: Data input to the flip-flop.
- **Output**:
  - `q`: Registered output (single bit, declared as `reg`).
- **Functionality**:
  - The flip-flop captures the value of `d` on the rising edge of `clk` and assigns it to `q`.
  - If `async_set` is high (on its rising edge), `q` is set to `1'b1` (1) regardless of the clock.
  - The set is asynchronous, meaning it does not depend on the clock signal.

### Code Explanation
```verilog
module dff_async_set ( input clk, input async_set, input d, output reg q );
```
- Declares the module with three inputs (`clk`, `async_set`, `d`) and one output (`q`).

```verilog
always @ (posedge clk, posedge async_set)
```
- The `always` block is sensitive to:
  - The positive (rising) edge of `clk`.
  - The positive (rising) edge of `async_set`.

```verilog
begin
    if (async_set)
        q <= 1'b1;
    else
        q <= d;
end
```
- If `async_set` is high (1), `q` is set to `1'b1` (1).
- Otherwise, on the rising edge of `clk`, `q` takes the value of `d`.
- The `<=` operator indicates non-blocking assignments, standard for sequential logic in Verilog.

### Key Characteristics
- **Asynchronous Set**: The set (`async_set`) takes precedence over the clock and can change `q` to `1` immediately when `async_set` goes high, without waiting for a clock edge.
- **Edge-Triggered**: The data (`d`) is captured and assigned to `q` only on the rising edge of `clk` when `async_set` is low.
- **Single-Bit Storage**: The flip-flop stores a single bit, as `q` and `d` are single-bit signals.

### Analysis of the verilog code for synchronous reset D Flip-Flop
Below is the verilog code for a synchronous reset Flip-Flop
```verilog
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
The provided Verilog code describes a D flip-flop (DFF) with a synchronous reset and an asynchronous reset input that appears to be unused. Let me analyze the module and explain its functionality, while addressing the presence of the `async_reset` input.

### Module Analysis
- **Module Name**: `dff_syncres`
- **Inputs**:
  - `clk`: Clock signal (triggers on positive edge).
  - `async_reset`: Asynchronous reset signal (active high, but unused in the code).
  - `sync_reset`: Synchronous reset signal (active high).
  - `d`: Data input to the flip-flop.
- **Output**:
  - `q`: Registered output (single bit, declared as `reg`).
- **Functionality**:
  - The flip-flop captures the value of `d` on the rising edge of `clk` and assigns it to `q`, unless `sync_reset` is high.
  - If `sync_reset` is high on the rising edge of `clk`, `q` is reset to `1'b0` (0).
  - The `async_reset` input is declared but not used in the logic, which may indicate an oversight or incomplete implementation.
- **Synchronous Reset**: The reset (`sync_reset`) only takes effect on the rising edge of `clk`, unlike an asynchronous reset.

### Code Explanation
```verilog
module dff_syncres ( input clk, input async_reset, input sync_reset, input d, output reg q );
```
- Declares the module with four inputs (`clk`, `async_reset`, `sync_reset`, `d`) and one output (`q`).

```verilog
always @ (posedge clk)
```
- The `always` block is sensitive only to the positive (rising) edge of `clk`. Notably, `async_reset` is not included in the sensitivity list, meaning it has no effect on the logic.

```verilog
begin
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;
end
```
- If `sync_reset` is high (1) on the rising edge of `clk`, `q` is set to `1'b0` (0).
- Otherwise, on the rising edge of `clk`, `q` takes the value of `d`.
- The `<=` operator indicates non-blocking assignments, standard for sequential logic in Verilog.

### Key Characteristics
- **Synchronous Reset**: The reset (`sync_reset`) only takes effect on the rising edge of `clk`, making it synchronous with the clock.
- **Unused `async_reset`**: The `async_reset` input is declared but not used in the logic. This could be a mistake or intentional (e.g., for future expansion). If an asynchronous reset was intended, the sensitivity list and logic would need modification (see below).
- **Edge-Triggered**: The data (`d`) is captured and assigned to `q` on the rising edge of `clk` when `sync_reset` is low.


### Iverilog compilation

Compile the design and testbench:

iverilog dff_asyncres.v tb_dff_asyncres.v

Run the simulation:

./a.out

View the waveform:

gtkwave tb_dff_asyncres.vcd

GTKWave Example 

![Screenshot_2025-05-30_10-45-13](https://github.com/user-attachments/assets/be8f6189-5cff-433b-8b00-d3637765c749)

> [!TIP]
> You can also compile and observe the waveforms of the asychronous reset/set synchronous reset/set verilog modules.

### Synthesis using Yosys
1. **Start Yosys**
    ```shell
    yosys
    ```

2. **Read the liberty library**
    ```shell
    read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

3. **Read the Verilog code**
    ```shell
    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/dff_asyncres.v
    ```

4. **Synthesize the design**
```shell
    synth -top dff_asyncres
```
5. **For Flip-Flops**
```shell
dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```



6. **Technology mapping**
    ```shell
    abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

7. **Visualize the gate-level netlist**
    ```shell
    show
    ```
![Screenshot_2025-05-30_11-03-00](https://github.com/user-attachments/assets/dfe3c231-a1f9-40c7-a2d8-fbbb2ca353ca)




    
