# Day 1 — Verilog RTL Design & Logic Synthesis

Understanding the role of Verilog, learning how to simulate circuits using Icarus Verilog (iverilog), analysing the design using gtkwave, and getting a first look at synthesis with Yosys. By the end of this session, you’ll know how to describe, simulate, and map a simple design into gates.

## Contents
[Design, Simulator & Testbench Basics](#1-design-simulator--testbench-basics)  

[Running Simulations with iverilog](#2-running-simulations-with-iverilog)  

[Hands-On: 2 bit Counter](#3-hands-on-2-bit-counter)  

[Walking Through the Verilog Code](#4-walking-through-the-verilog-code)  

[Synthesis with Yosys & Cell Libraries](#5-introduction-to-yosys--gate-libraries)  

[Lab: Mapping the Mux to Gates](#6-synthesis-lab-with-yosys)  


## Key Takeaways

### 1. Design, Simulator & Testbench Basics
**Simulator**

A simulator is software that mimics how your circuit behaves when inputs are applied. It lets you verify functionality long before building the chip.

**Design**

The design is your RTL description — Verilog code that represents the actual circuit behavior.

**Testbench**

The testbench is a wrapper around the design. It feeds test inputs, observes outputs, and ensures correctness.
<img width="971" height="532" alt="Screenshot from 2025-09-21 18-45-05" src="https://github.com/user-attachments/assets/3fbda2b4-8b63-44a4-b714-ebd0df5dedff" />

### 2. Running Simulations with iverilog

**iverilog** is a popular open-source simulator for Verilog.

- The design and its testbench are compiled together.

- The simulator runs and can produce a .vcd waveform file.

- You can view this .vcd in GTKWave to inspect signals cycle by cycle.

### 3. Hands-On: 2 bit Counter

Let’s begin with a simple 2 bit counter.

- Step 1: Get the Code

Clone the repo (or create your own working folder):
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
 - Step 2: Compile & Run
```bash
iverilog good_counter.v tb_good_counter.v
./a.out
```
 - Step 3: View the Waveform
```bash
gtkwave tb_good_counter.vcd
```
<img width="1210" height="773" alt="Screenshot from 2025-09-21 17-41-07" src="https://github.com/user-attachments/assets/290b4798-b5b0-424d-89f3-e477f2cfa015" />

### 4. Walking Through the Verilog Code

Here’s the code for the 2-bit good_counter:
```v
module good_counter (input clk , input reset , output reg [1:0] cnt);
wire comp;

assign comp = (cnt == 2'b10);

always @(posedge clk , posedge reset)
begin
        if(reset)
                cnt <= 2'b00;
        else if(comp)
                cnt <= 2'b00;
        else
                cnt <= cnt+1;
end
endmodule
```
- Inputs: clk (clock), reset (asynchronous reset).

- Output: cnt → 2-bit register storing the counter value.

- Wire: comp → high when cnt == 2.

Logic Flow:

-On reset, counter clears to 00.

- If cnt reaches 10 (2 in decimal), it wraps back to 00.

- Otherwise, it increments on each clock edge.

This makes the design a mod-3 counter that cycles: 00 → 01 → 10 → 00 → …

---
## 5. Synthesis with Yosys & Cell Libraries

### Yosys in a Nutshell

**Yosys** is an open-source tool that converts Verilog into a gate-level netlist.  
It performs:

- **Synthesis:** RTL → logic network  
- **Optimization:** Improves timing/area  
- **Technology Mapping:** Matches logic to real standard cells  
- **Verification:** Ensures correctness  

---

### Why Do Libraries Have Multiple Gate Versions?

Standard-cell libraries contain several versions of the same logic gate, each optimized differently:

- **Performance:** Faster vs. slower  
- **Power:** Low-power vs. high-power  
- **Area:** Small area vs. large area  
- **Drive Strength:** Stronger or weaker drive capab
---
