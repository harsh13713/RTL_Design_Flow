# Day 1 — Verilog RTL Design & Logic Synthesis

Understanding the role of Verilog, learning how to simulate circuits using Icarus Verilog (iverilog), analysing the design using gtkwave, and getting a first look at synthesis with Yosys. By the end of this session, you’ll know how to describe, simulate, and map a simple design into gates.

## Contents

[Design, Simulator & Testbench Basics](#1.-Design,-Simulator-&-Testbench-Basics)

[Running Simulations with iverilog](#2.-Running-Simulations-with-iverilog)

[Hands-On: 2:1 Multiplexer](#3.-Hands-On:-2-bit-Counter)

[Walking Through the Verilog Code](#4.-Walking-Through-the-Verilog-Code)

[Synthesis with Yosys & Cell Libraries](#5.-Synthesis-with-Yosys-&-Cell-Libraries)

[Lab: Mapping the Mux to Gates](#6.-Lab:-Mapping-the-Mux-to-Gates)

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
4. Walking Through the Verilog Code

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

## 5. Introduction to Yosys & Gate Libraries

###  What is Yosys?

**Yosys** is a powerful open-source synthesis tool for digital hardware. It takes your Verilog code and converts it into a gate-level netlist—a hardware blueprint.

#### Yosys Features

- **Synthesis:** Converts HDL to a logic circuit
- **Optimization:** Improves speed or area
- **Technology Mapping:** Matches logic to actual hardware cells
- **Verification:** Checks correctness
- **Extensibility:** Supports custom flows

###  Why Do Libraries Have Different Gate "Flavors"?

A `.lib` file contains many versions of each gate (like AND, OR, NOT) with different properties:

- **Performance:** Faster gates for critical paths, slower for power savings
- **Power:** Some gates use less energy
- **Area:** Smaller gates for compact chips
- **Drive Strength:** Stronger gates to drive more load
- **Signal Integrity:** Specialized gates for noise/performance
- **Mapping:** Synthesis tools pick the best flavor for your needs

---

## 6. Synthesis Lab with Yosys

Let’s synthesize the `good_mux` design using Yosys!

###  Step-by-Step Yosys Flow

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
    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
    ```

4. **Synthesize the design**
    ```shell
    synth -top good_mux
    ```

5. **Technology mapping**
    ```shell
    abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

6. **Visualize the gate-level netlist**
    ```shell
    show
    ```

<div align="center">
  <img src="https://github.com/user-attachments/assets/4b3a9939-92d0-4efc-ad69-e96faf19e6c3" alt="Yosys Gate-level Schematic" width="70%">
</div>

---

## 7. Summary

- You learned about simulators, designs, and testbenches.
- You ran your first Verilog simulation with iverilog and visualized waveforms.
- You analyzed the 2-to-1 mux code.
- You explored Yosys and learned why gate libraries have various flavors.


---
