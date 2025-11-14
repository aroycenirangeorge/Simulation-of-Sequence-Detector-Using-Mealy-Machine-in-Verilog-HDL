# **Simulation of Sequence Detector Using Mealy Machine in Verilog HDL**

---

## **Aim**
To design and simulate a **Sequence Detector using Mealy Machine** in Verilog HDL that detects a specific bit pattern from a given binary input sequence.

---

## **Apparatus Required**
- Computer with **Vivado Design Suite** installed  
---

## **Theory**

A **Sequence Detector** identifies a specific sequence of bits from a stream of binary input data.  
**Mealy Machine** – Output depends on both the current state and the input.

In the **Mealy Machine**, the output can change immediately in response to an input, making it faster than the Moore type.  
The Mealy model uses **state transitions** and **output logic** based on both the current state and the input signal.

For example, a **sequence detector** that detects `"1011"`:
- It produces an output `1` whenever the sequence `1011` appears in the input stream.
- Overlapping sequences are also detected.

---
## State Diagram 
<img width="363" height="550" alt="image" src="https://github.com/user-attachments/assets/7861102e-c301-422a-bfa8-d8d887a8652b" />

## Verilog Code
```verilog
module mealy_sequence(clk, rst, in, out);
    input clk, rst, in;
    output reg out;
    parameter A = 2'b00,
              B = 2'b01,
              C = 2'b10,
              D = 2'b11;
    reg [1:0] current_state, next_state;

    always @(posedge clk or posedge rst) begin
        if (rst)
            current_state <= A;
        else
            current_state <= next_state;
    end

    always @(*) begin
        case (current_state)
            A: next_state = (in == 0) ? A : B;
            B: next_state = (in == 1) ? B : C;
            C: next_state = (in == 0) ? A : D;
            D: next_state = (in == 1) ? B : A;
            default: next_state = A;
        endcase
    end

    always @(*) begin
        out = (current_state == D) ? 1 : 0;
    end
endmodule
```
## Testbench
```verilog
module mealy_sequence_tb;

    reg clk, rst, in;
    wire out;
    
    mealy_sequence uut (clk,rst,in,out);
    
    initial  clk = 0;
    always #5 clk = ~clk;   
    initial 
    begin
    in=0;
    rst=0;
    in=1;#10;
    in=0;#10;
    in=1;#10;
    in=0;#10;
    in=0;#10;
    in=1;#10;
    in=0;#10;
    in=1;#10;
    $finish;
    end
endmodule
```
## Simulation Output 
---

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/0193c8a7-b8af-4429-a958-333258b5dac0" />

---
## Result

The Mealy Machine Sequence Detector for the bit pattern "11011" was successfully designed and simulated using Verilog HDL.
Simulation results confirm that the circuit correctly detects the pattern and supports overlapping sequences.

