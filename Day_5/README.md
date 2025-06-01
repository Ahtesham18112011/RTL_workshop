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














