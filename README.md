# -Advanced-Physical-Design-using-OpenLANE-Sky130-an-ASIC-Design


Introduction to Openlane

Invoking Openlane

docker

 docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
 
 /openlane$docker
 

bash-4.2$ ./fow.tcl -interactive

%package require openlane 0.9

%prep -design picorv32a

