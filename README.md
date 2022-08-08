# -Advanced-Physical-Design-using-OpenLANE-Sky130-an-ASIC-Design

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


![183133312-e8dfc8f6-470b-4d4e-9796-84c8215c6e28](https://user-images.githubusercontent.com/71349997/183375337-be1ebd6e-8065-4bfa-af5a-53c0a3bee611.png)







