# -Advanced-Physical-Design-using-OpenLANE-Sky130-an-ASIC-Design

Day- 1

Introduction to Openlane

Invoking Openlane

docker

 docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
 
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
