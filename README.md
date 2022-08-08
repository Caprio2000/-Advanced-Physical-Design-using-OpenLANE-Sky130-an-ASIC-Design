# -Advanced-Physical-Design-using-OpenLANE-Sky130-an-ASIC-Design

Day- 1

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


Day-2

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

Day -3

Cloninng repo in openlane directory

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

magic -T sky130A.tech sky130_inv.mag


![182999348-caf44fd9-30d4-443f-bdcf-435666250456](https://user-images.githubusercontent.com/71349997/183374691-912111ea-295f-48bd-99bf-a4bc861ded4d.jpg)
