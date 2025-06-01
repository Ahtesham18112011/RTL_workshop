# Day 5: Optimization in syntheis
Welcome to the Da 5 of this workshop! This day deals with if-else statements in Verilog and then discusses about for loops and generate blocks in verilog HDL.
## What are if-else statements in verilog?
In Verilog, **if-else statements** are used for conditional execution in behavioral modeling, typically within procedural blocks like `always`, `initial`, or in tasks and functions. They allow the code to execute different blocks of code based on whether a condition evaluates to true or false. Verilog’s if-else syntax is similar to that of other programming languages like C, but it’s tailored for hardware description with specific considerations for synthesis.

### Syntax
```verilog
if (condition) begin
    // Code block executed if condition is true
end
else begin
    // Code block executed if condition is false
end
```

- **condition**: An expression that evaluates to true (non-zero) or false (zero).
- **begin ... end**: Used to group multiple statements. If only one statement is present, `begin` and `end` can be omitted.
- The `else` part is optional.

### Nested if-else
You can nest if-else statements or use `else if` for multiple conditions:
```verilog
if (condition1) begin
    // Code for condition1 true
end
else if (condition2) begin
    // Code for condition2 true
end
else begin
    // Code if no conditions are true
end
```
---

# What are inferred Latches in Verilog?
In Verilog, **inferred latches** occur during synthesis when a combinational logic block (e.g., in an `always` block) does not assign a value to a variable in all possible execution paths. This leads the synthesis tool to infer a latch to "hold" the previous value of the variable, as it assumes the variable needs to retain its state when not explicitly updated. Latches are generally undesirable in combinational logic designs because they introduce sequential behavior, which can cause timing issues, increase power consumption, and complicate verification.

### Why Latches Are Inferred
Latches are inferred when:
1. A variable in a combinational `always` block (e.g., `always @(a, b)`) is not assigned a value in every possible path of execution (e.g., missing `else` in an if-else statement).
2. The synthesis tool assumes the variable must "remember" its previous value, implying a latch.

### Example of Latch Inference
```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        if (sel == 1'b1)
            y = a;
        // No else clause, so y is not assigned when sel == 0
    end
endmodule
```
- **Problem**: When `sel` is 0, `y` is not assigned. The synthesis tool infers a latch to hold `y`’s previous value.
- **Result**: The output `y` behaves like a latch, which was likely not the design intent for a combinational circuit.

To solve this problem we can add the else statement, or add the **default** keyword, by adding a default statement the example will look like:-
```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
case(sel)
       1'b1 : y = a;
       default : y = 1'b0; //default to 0    
endcase
end
endmodule
```

## Labs for if-else/case statements
### Lab 1
The verilog code for lab 1 is provide below:-
```verilog
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
end
endmodule
```

![in_comp_if](https://github.com/user-attachments/assets/91d1cc1d-bb3a-4ea8-a272-363414777868)


### Lab 2
The lab 2 is the synthesis of the above verilog:-

![incomp_synth](https://github.com/user-attachments/assets/691045e7-39e0-4e6c-98bd-511b501fbe35)


### Lab 3
The verilog code for lab 3 is given below:-
```verilog

module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule
```

![icomp2](https://github.com/user-attachments/assets/2f614956-e4af-4d29-80ae-13a167e7831d)


### Lab 4
The lab 4 is the synthesis of the above verilog code

![incomp2synth](https://github.com/user-attachments/assets/880ff7bb-23fb-4362-bf8f-a2494a854b53)


### Lab 5
The verilog code for lab 5 is  given below:-
```verilog


module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
		default : y = i2;
	endcase
end
endmodule
```

![compcase](https://github.com/user-attachments/assets/cfe97c45-a487-4f06-b4a2-74b3a61bee14)


### Lab 6
The lab 6 is the synthesis of the above verilog code

![compcase_synth](https://github.com/user-attachments/assets/8c871511-6e55-4e80-be11-86e9efd87cad)


### Lab 7
The verilog code  for lab 7 is given below:-
```verilog
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end

endmodule
```
![badcase](https://github.com/user-attachments/assets/4ccf37aa-5502-4750-bedb-9b2ec0748a53)


### Lab 8
The verilog code  for lab 8 is given below:-
```verilog
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule
```
![Screenshot_2025-05-28_12-39-30](https://github.com/user-attachments/assets/3f6068f3-726d-4192-b3cd-f88b3611e752)

> [!IMPORTANT]
> Steps to perform the above labs are already shown in [Day 1](https://github.com/Ahtesham18112011/RTL_workshop/tree/main/Day_1).


## For loop in verilog
In Verilog, a **for loop** is a procedural statement used within **procedural blocks** (like `initial`, `always`, or tasks/functions) to execute a set of statements multiple times based on a loop counter. It is primarily used for repetitive tasks, such as initializing arrays, generating patterns, or iterating over indices in simulation or synthesis.

### Syntax of a For Loop in Verilog
The general syntax of a `for` loop in Verilog is:

```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```

- **Initialization**: Sets the initial value of the loop counter (e.g., `integer i = 0`).
- **Condition**: A boolean expression checked before each iteration (e.g., `i < N`). The loop continues as long as the condition is true.
- **Increment**: Updates the loop counter after each iteration (e.g., `i = i + 1` or `i++`).
- **Statements**: The block of code executed in each iteration, enclosed in `begin` and `end` if multiple statements are included.

### Key Characteristics
- **Procedural Context**: For loops must appear inside procedural blocks like `initial`, `always`, tasks, or functions. They cannot be used in continuous assignments (`assign`) or module instantiations directly.
- **Synthesis**: For loops are synthesizable if they have a fixed number of iterations (statically determinable at compile time) and describe hardware behavior (e.g., unrolled logic for combinational or sequential circuits). Loops with dynamic bounds or non-hardware-like behavior (e.g., delays) may not synthesize.
- **Loop Counter**: Typically uses an `integer` or `reg` variable as the counter. The counter must be declared before the loop unless it’s a genvar in generate blocks (see below for generate context).

### Example
### Verilog Code for 4-to-1 MUX Using For Loop

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data, // 4 input data lines (d0, d1, d2, d3)
    input wire [1:0] sel,  // 2-bit select signal
    output reg y           // Output
);
    integer i;
    always @(data, sel) begin
        y = 1'b0; // Default output
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel) begin
                y = data[i];
            end
        end
    end
endmodule
```

### Explanation
- **Inputs**:
  - `data`: A 4-bit vector (`data[0]` to `data[3]`) representing the four input lines (d0, d1, d2, d3).
  - `sel`: A 2-bit select signal (`sel[1:0]`) that chooses which input to pass to the output (00, 01, 10, or 11).
- **Output**:
  - `y`: A single-bit output (declared as `reg` because it’s assigned in an `always` block).
- **For Loop Logic**:
  - The `always @(data, sel)` block ensures the logic updates whenever `data` or `sel` changes, making it combinational.
  - The loop iterates over `i` from 0 to 3 (covering all four inputs).
  - The `if (i == sel)` condition checks if the loop index matches the select signal. If true, `y` is assigned `data[i]`.
  - The default value `y = 1'b0` ensures `y` has a defined value before the loop, though it will be overwritten when `i == sel`.
- **Functionality**:
  - When `sel = 00` (0), `y = data[0]`.
  - When `sel = 01` (1), `y = data[1]`.
  - When `sel = 10` (2), `y = data[2]`.
  - When `sel = 11` (3), `y = data[3]`.
- **Synthesis**:
  - The loop is synthesizable because it has a fixed number of iterations (4), and the synthesis tool unrolls it into equivalent combinational logic (effectively a multiplexer).
  - The loop is equivalent to a case statement or conditional logic but uses iteration to check the select condition.

## Generate Block in verilog
A **generate block** in Verilog is used to dynamically create hardware structures, like module instances or logic, during compilation. Enclosed in `generate` and `endgenerate`, it supports `for` loops, `if`, or `case` statements to create repetitive or conditional code based on parameters. A `genvar` variable controls loops, and the construct is evaluated at elaboration, not simulation, enabling scalable and reusable designs.

### Example
```verilog
genvar i;
generate
  for (i = 0; i < 4; i = i + 1) begin : gen_loop
    and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
  end
endgenerate

```
## What is `rca` (Ripple Carry Adder)?

RCA is a simple adder in digital electronics which adds binary bits, which is made up of Full Adders, Addition of `n` bits requires `n` Full Adders, each output carry is connected to the next Full Adder's carry input. Below is the diagram of a Ripple carry Adder:-

![image](https://github.com/user-attachments/assets/f1ec27d4-b770-4d7a-a418-6435fc81f538)

## Labs on Loop and generate blocks
### Lab 9
The verilog code  for lab 8 is given below
```verilog
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule
```
The Verilog module mux_generate is a 4-to-1 multiplexer. It selects one of four single-bit inputs (i0, i1, i2, i3) based on a 2-bit select signal (sel) and assigns it to the output y. The inputs are concatenated into a 4-bit wire i_int = {i3, i2, i1, i0}. An always block with a for loop checks if the loop index k matches sel and assigns i_int[k] to y.

![mux_generate](https://github.com/user-attachments/assets/80789638-c349-44a9-92f4-7597d5925c63)

### Lab 10
The verilog code for lab 10 is given below:-
```verilog
module demux_case (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
	case(sel)
		3'b000 : y_int[0] = i;
		3'b001 : y_int[1] = i;
		3'b010 : y_int[2] = i;
		3'b011 : y_int[3] = i;
		3'b100 : y_int[4] = i;
		3'b101 : y_int[5] = i;
		3'b110 : y_int[6] = i;
		3'b111 : y_int[7] = i;
	endcase

end
endmodule

```
 

- A `reg [7:0] y_int` is used to store intermediate values.
- The `always @(*)` block resets `y_int` to `8'b0` and uses a `case` statement to set one bit of `y_int` to the value of `i` based on `sel` (e.g., `sel = 3'b000` sets `y_int[0] = i`).
- The outputs `{o7, o6, o5, o4, o3, o2, o1, o0}` are assigned from `y_int`.
- Only the output selected by `sel` gets the value of `i`; all others are `0`.

![demux-case](https://github.com/user-attachments/assets/1836a255-e260-47de-9a8e-45899b19fc03)


### Lab 11
The verilog code for lab 11 is given below:-

```verilog

module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule
```
The Verilog module `demux_generate` is an 8-to-1 demultiplexer. It takes a single-bit input `i` and a 3-bit select signal `sel` to route `i` to one of eight single-bit outputs (`o0` to `o7`).

- A `reg [7:0] y_int` holds intermediate values.
- The `always @(*)` block resets `y_int` to `8'b0` and uses a `for` loop to set `y_int[k] = i` when `k` matches `sel`.
- The outputs `{o7, o6, o5, o4, o3, o2, o1, o0}` are assigned from `y_int`.
- The output selected by `sel` gets the value of `i`; others are `0`.

![demux-generate](https://github.com/user-attachments/assets/a5a2c004-a16f-44cd-8d80-c23f1c932e6c)


### Lab 12
The verilog codes for lab 12 are given below:-
```verilog
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule

```
The Verilog module `rca` is an 8-bit ripple-carry adder. It takes two 8-bit inputs (`num1`, `num2`) and produces a 9-bit output (`sum`).

- A `generate` block with a `for` loop instantiates seven full adders (`fa`) for bits 1 to 7, connecting `num1[i]`, `num2[i]`, and the carry-out from the previous stage (`int_co[i-1]`) to produce `int_sum[i]` and `int_co[i]`.
- A separate full adder (`u_fa_0`) handles bit 0 with a carry-in of `0`.
- The 8-bit `int_sum` is assigned to `sum[7:0]`, and the final carry-out (`int_co[7]`) is assigned to `sum[8]`.
- Adds two 8-bit numbers, producing an 8-bit sum and a carry-out bit.

```verilog
module fa (input a , input b , input c, output co , output sum);
	assign {co,sum}  = a + b + c ;
endmodule
```
This is a simple module for a Full-Adder which sums 3 numbers.

![rca_org](https://github.com/user-attachments/assets/1d8876f9-e303-4a73-945e-97756a37bb73)

---
> [!IMPORTANT]
> Steps to perform the above labs are already shown in [Day 1](https://github.com/Ahtesham18112011/RTL_workshop/tree/main/Day_1).



















