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


