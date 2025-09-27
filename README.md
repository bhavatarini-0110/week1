# Topics Covered

Simulator, Design, and Testbench — what they mean

Running your first Verilog program with Icarus Verilog

Lab exercise: 2-to-1 multiplexer

Code breakdown and explanation

Introduction to Yosys and cell libraries

Synthesis workflow with Yosys

Recap

1. Simulator, Design, and Testbench

Simulator: A tool that mimics how your circuit would behave in real life. You give it input signals and it shows you the outputs. Great for verifying logic before building hardware.

Design: The Verilog module that describes the digital logic you want to implement.

Testbench: A “virtual lab environment” that feeds different inputs to the design and checks how it responds.
<img width="1515" height="852" alt="448241377-93927b96-df80-4da5-b801-284fc2cc6757" src="https://github.com/user-attachments/assets/d5d11a0e-e870-490d-99d1-32304ddc7ea6" />

Together: Design + Testbench → Simulator → Output waveforms

2. First Look at Icarus Verilog

Icarus Verilog (iverilog) is an open-source simulator for Verilog HDL.
Simulation flow looks like this:

Provide design and testbench files to iverilog.

The tool compiles them into an executable (a.out).

Running a.out generates a VCD (Value Change Dump) file.

Use GTKWave to view signal waveforms.
<img width="1419" height="771" alt="448242876-3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" src="https://github.com/user-attachments/assets/57e336db-c386-49aa-a129-aa63a7e26322" />


3. Lab Exercise: Simulate a 2:1 Multiplexer




Step 1: Get the files

Step 2: Install required tools

sudo apt install iverilog
sudo apt install gtkwave



Step 3: Run simulation

iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
<img width="1018" height="260" alt="image" src="https://github.com/user-attachments/assets/b08d5e09-a7f7-443d-a5b1-87144899a774" />



Now you should see the multiplexer signals in GTKWave

4. Verilog Code Walkthrough

Here’s the multiplexer (good_mux.v):

module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule




5. Yosys and Gate Libraries

Yosys is a framework for digital synthesis. It takes your RTL and turns it into a gate-level netlist (closer to physical hardware).

Synthesis: RTL → logic circuit

Optimization: Reduce area/power or improve speed

Mapping: Match your circuit to real hardware cells from the library (.lib)

Why multiple gate versions?
A standard cell library provides variations for speed, size, power, and drive strength. The synthesis tool picks the best option depending on design needs.

6. Yosys Synthesis Flow (Hands-on)
yosys


Inside Yosys, run:

# Load technology library
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib  

# Load design
read_verilog good_mux.v  

# Synthesize
synth -top good_mux  

# Map to cells
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib  

# View schematic
show


This gives you a gate-level schematic of the mux.
![WhatsApp Image 2025-09-27 at 18 03 29_54743218](https://github.com/user-attachments/assets/ef4ae06d-ad4a-43f5-9ccc-9e2b9c6dc6da)

7. Wrap-Up

By the end of Day 1, you should now be comfortable with:
✅ Difference between simulator, design, and testbench
✅ Running Verilog simulation with iverilog & GTKWave
✅ Understanding a simple multiplexer design
✅ Basics of Yosys synthesis and cell libraries
