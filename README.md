# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,
```
◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

◦ SDC (Synopsis Design Constraint) File (.sdc)
timescale 1ns/1ns
module counter(clk,m,rst,count);
input clk,m,rst;
output reg [3:0] count;
always@(posedge clk or negedge rst)
begin
if (!rst)
count=0;
else if(m)
count=count+1;
else
count=count-1;
end
endmodule
```
```
timescale 1ns/1ns
module counter_test;
reg clk,rst,m;
wire [3:0] count;
initial
begin
clk=0;
rst=0;#5;
rst=1;
end
initial
begin
m=1;
#160 m=0;
end
counter dut(clk,m,rst,count);
always #5 clk=~clk;
initial $monitor("Time=%t rst=%b clk=%b count=%b" , $time,rst,clk,count);
initial
#320 $finish;
endmodule
```
```
read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib
read_hdl counter.v
elaborate
read_sdc counter_con.sdc 
syn_generic
report_area
syn_map
report_area
syn_opt
report_area 
report_area > counter_area.txt
report_power > counter_power.txt
report_cells > counter_cell.rpt
write_hdl >counter_netlist.v
gui_show
```

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

![WhatsApp Image 2025-05-19 at 16 04 28_007e75be](https://github.com/user-attachments/assets/3931cc78-cadf-4620-bd21-aca97849614c)

#### Area report:
![WhatsApp Image 2025-05-19 at 16 04 29_67e9dd47](https://github.com/user-attachments/assets/94c74fd1-b6b2-4f05-894c-ddaa7afc2990)

#### Power Report:
![WhatsApp Image 2025-05-19 at 16 04 28_bab3ae80](https://github.com/user-attachments/assets/425adc27-9b9a-408f-b85d-7477219b4592)

#### Timing Report: 
![WhatsApp Image 2025-05-19 at 16 04 29_a4d1998f](https://github.com/user-attachments/assets/ac9d019e-ba1c-40d5-8f22-a8035534cd41)

#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





