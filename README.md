# -Advanced-Physical-Design-using-OpenLANE-Sky130-an-ASIC-Design
contents


Day1 – Inception of open-source EDA, OpenLANE and Sky130 PDK

How to talk to computers
SoC design and OpenLANE
Starting RISC-V SoC Reference design
Get familiar to open-source EDA tools


Day 2 - Understand importance of good floorplan vs bad floorplan and introduction to library cells

Chip Floor planning considerations
Library Binding and Placement
Cell design and characterization flows
General timing characterization parameters


Day 3 - Design and characterize one library cell using Magic Layout tool and ngspice

Labs for CMOS inverter ngspice simulations
Inception of Layout – CMOS fabrication process
Sky130 Tech File Labs

Day 4 - Pre-layout timing analysis and importance of good clock tree

Timing modelling using delay tables
Timing analysis with ideal clocks using openSTA
Clock tree synthesis TritonCTS and signal integrity
Timing analysis with real clocks using openSTA

Day 5 - Final steps for RTL2GDS

Routing and design rule check (DRC)
PNR interactive flow tutorial



**Day- 1**

Introduction to Openlane

Invoking Openlane

docker

 /openlane$docker
 

bash-4.2$ ./fow.tcl -interactive

%package require openlane 0.9

%prep -design picorv32a


Number of cells - 14876
Total Number of flops - 1613
Flop count = Total Number of flops/ Number of cells =0.1084
~ 10.84% of flops used .
Buffer count - 1656
Buffer count = No. of buffer/Number of cell = 0.1113
~ 11.13 % buffer used.








**Day-2**

floorplan and Introduction to Library cells

Defining height and width of die and core

Utilization factor =Area occupied by netlist/ Total area of core

Utilization factor is kept about 0.6 to 0.5 or even lower (why? for future scope of adding cells into it)

Aspect ratio = Heightof die/ Width of die


$docker



run_floorplan

Successful floorplanning results in a .def file as output.

This file contains the die area and placement of standard cells.

DIEAREA in microns is 660.685x 671.405 = 443587.2

Floorplan



Proper mapping of Library and tech file is important to reflect the same in magic layout

Magic Layout Tool is used for visualizing the layout after successfull floorplan.

Three files are required for viewing the layout

1. Technology File (sky130A.tech)
2. Merged LEF file (merged.lef)
3. DEF File





**Day -3**

Cloninng repo in openlane directory

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

magic -T sky130A.tech sky130_inv.mag


![182999348-caf44fd9-30d4-443f-bdcf-435666250456](https://user-images.githubusercontent.com/71349997/183374691-912111ea-295f-48bd-99bf-a4bc861ded4d.jpg)

Extract SPICE Netlist from Layout in Magic
This is done in 2 steps.

Create the extraction file using

extract all
This will create a .ext file and using which we can generate the spice file using the following commands to be used in ngspice for simulation.

ext2spice cthresh 0 rthresh 0
ext2spice
![183026065-1307a83c-2c1d-4268-966a-74ab932c30fb](https://user-images.githubusercontent.com/71349997/183374971-9c98b84e-d96a-4bd6-b18d-b124666b579b.png)


spice file 


![183027987-d44be707-eef7-448f-87ec-ddf24a8fee84](https://user-images.githubusercontent.com/71349997/183375037-cbc1a6a0-4e2e-4591-9345-ca9b7983829d.png)

editing of spice file 

.option scale= 0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M0 Y A VGND VGND nshort_model.0 ad=0 pd=0 as=0 ps=0 w=35 l=23
M1 Y A VPWR VPWR pshort_model.0  ad=0 pd=0 as=0 ps=0 w=37 l=23

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.07fF
C1 A Y 0.05fF
C2 Y VPWR 0.11fF
C3 Y VGND 0.24fF
C4 VPWR VGND 0.59fF
//.ends


.tran 1n 20n

.control
run
.endc
.end

ngspice sky130_inv.spice


![183133768-06e13727-0820-41e8-97e8-e6a26ac73bfb](https://user-images.githubusercontent.com/71349997/183375211-6338c937-7266-4eb8-adfb-ebe530a6022c.png)

ploting y vs time a in ng spice

![183133312-e8dfc8f6-470b-4d4e-9796-84c8215c6e28](https://user-images.githubusercontent.com/71349997/183375337-be1ebd6e-8065-4bfa-af5a-53c0a3bee611.png)

calculations 

1. Fall delay Time between input rise to output fall

![183134230-fe059e6c-6d65-4e65-9a11-307e98844f01](https://user-images.githubusercontent.com/71349997/183375790-626fdcef-a8ca-4058-87d8-398af0131c81.png)

![183134460-09b2ed60-0374-4f29-b2cd-45f55a9122ac](https://user-images.githubusercontent.com/71349997/183375807-227aca48-c5e5-4311-b370-8e382b7e0b99.png)

tdf = 0.003 ns
2. Rise delayime between input fall to output rise

![183135025-e3cb6504-3b94-47cd-866f-e346da967975](https://user-images.githubusercontent.com/71349997/183375992-9d05cd84-c5f2-40f0-b3e2-a9e80305c562.png)

![183135112-a652c18d-cb69-4c6a-b902-f4b8b1c28656](https://user-images.githubusercontent.com/71349997/183376025-29932cca-9b65-4cfe-8cf1-dc6bd34b9cba.png)

tdf = 0.028ns





**Day-4**

Pre-layout timing analysis and importance of clock tree

Magic to Standard Cell LEF Generation
PnR is possible just by giving information about the pin placement and metal information, there is no need of providing any information about the logic. This is done by the LEF file (Library Exchange Format) to perform interconnect routing in conjunction to routing guides generated from the PnR flow. This is how the companies do not disclose the logic information to the foundry.

Before generating the LEF file for our standard cell design we need to ensure that the design we have made is satisfing the foundry requirment i.e. track details. This we can confirm by making a grid in magic with the proper details of the tracks from track.info file
![183233728-7fbe1a5b-8c89-4a4d-9897-4e0cc9bc183f](https://user-images.githubusercontent.com/71349997/183387206-c5b3ef95-7780-4975-a4fb-988db5a4e016.png)

<layer-name> <X/Y direction> <track-offset> <track-pitch>
 
 
The same info has to be passed to magic tool to form the grid.To create a standard cell LEF from an existing layout, some important aspects need to be taken into consideration.
 
 Now run the following command

sta pre_sta.conf
you can observe following results
 
 ![183260551-c122a5e0-d13c-4cca-8099-d07661fd461d](https://user-images.githubusercontent.com/71349997/183407011-586b6f70-0e55-4774-95e5-6a5e21c80f25.png)

 
After this we can run floorplan and placement as done before to see the placed design in the magic again.

Clock Tree Synthesis
Run the following command to run the CTS in the previous design.

run_cts
In the synthesis folder we can see another new netlist as follows: 
 

 image Both the netlist files are different as the one before CTS does not contain the clock b uffers but the one aftyer cts has all the clock buffer information as well.
 

Post-CTS STA Analysis
OpenLANE has the OpenROAD application integrated into its flow. The OpenROAD application has OpenSTA integrated into its flow. Therefore, we can perform STA analysis from within OpenLANE by invoking OpenROAD. So in the openlane execute the following command to open openroad in openlane

openroad
Read lef and def files after CTS and make the db.

Reading LEF:
read_lef /openLANE_flow/designs/picorv32a/runs/03-08_10-12/tmp/merged.lef
Reading DEF:
read_def /openLANE_flow/designs/picorv32a/runs/03-08_10-12/results/cts/picorv32a.cts.def
lef file will not change untill you change the technology but the def files change depending on how you add different cells to your design. 3. Writing a DB:

write_db pico_cts.db
Read the same DB:
read_db pico_cts.db
Read the post_cts netlist:
read_verilog /openLANE_flow/designs/picorv32a/runs/03-08_10-12/results/synthesis/picorv32a.synthesis_cts.v
6.1. Set the FAST and SLOW library

read_liberty -max $::env(LIB_SLOWEST)
read_liberty -min $::env(LIB_FASTEST)
(or) 6.2. Set the Typical library

read_liberty  $::env(LIB_SYNTH_COMPLETE)
Read sdc
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
Set propagated clocks
set_propagated_clock [all_clocks]
Check the reports
report_checks -path_delay min_max -format full_clock_expanded -digits 4
The above process checks the design taking the real clocks into consideration. After this if the thing go correct then timings met will be shown.

 


![183276403-5177d5fc-20f0-4af3-9cc0-ae3a3a186fca](https://user-images.githubusercontent.com/71349997/183407259-a1e273c3-2791-4fe7-92f5-a0d9bc958e54.png)


 **DAY_5**
 
 



Global and Detailed RoutingOpenLANE uses FastRoute as global and TritonRoute as the detailed routing engine for physical implementations of designs. Routing consists of two stages:

Global Routing - first partitions the routing region into tiles and decides tile-to-tile paths for all nets. 
 
 Detailed Routing - the exact tracks and vias for nets after the routing guide is provided.


 In openlane these all steps are done by simple execution of following command

run_routing


The follwoing image shows routing has been completed without any DRC violation.
 ![183288422-3721236c-0b1a-4923-9497-1f6079f65b7d](https://user-images.githubusercontent.com/71349997/183407997-094d682c-d748-422e-8ac8-00cdc4e6baa3.png)

 

SPEF ExtractionAfter routing we need to get the information about the parasitic information of the routed path as these will also impact the STA analysis. SPEF file is needed to perform sign-off post-route STA analysis. SPEF Extractor has been included in the openlane flow. Following image shows that SPEF file has been generated. There is no need to execute any specific command for this extraction. This file is automatically being generated at the end of routing process.

![183289146-4e31861c-d177-439f-8d46-2cc9e810e4aa](https://user-images.githubusercontent.com/71349997/183408028-32c613fe-5bb1-4276-86d1-1c6c9bbdf7bb.png)
 
 Special Thanks to 
 
 *VSD SYSTEM DESIGN
 *OpaneLane
 
 
 
 Acknowledgements
 
Kunal Ghosh
 
Nickson Jose
 
ShonTaware
 
Grant Brown
 
 
