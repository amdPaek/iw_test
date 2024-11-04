## Instrumentation wrapper (IW) + FINN IP
This is a fork from finn-example (https://github.com/Xilinx/finn-examples/tree/main), to incorporate IW with a FINN IP (produced by FINN Compiler) to measure latency and throughput by running the model on Alveo U250. 

### Directory structure
The overall directory structure is the same as finn-example. What has been added is "iw" directory, which contains all the finn example models with a modified build script (build.py). Each model directory (in "iw") contains "sw" directory, where one can build xclbin for Vitis, and compile/run the host software.

### Environment setup
Make sure vitis/vivado and python environments are installed correctly. Vitis version 2023.1 is used for this project.
In order to run this on U250, make sure the XRT is installed, and Alveo U250 is installed (with right shell that works with a Vitis/XRT version), flashed, and verified

### Steps 

1) Set the environment \
See "set_2023.1" script and modify it to your environment
```
cd <top_dir>/build/; source get-finn.sh # to install the right version of finn compiler
mv iw_gtsrb finn  # script to build gtsrb model
```

2) Generate xo files for FINN IP and IW \
The following shown for "gtsrb" model. Apply the same process for other models.
```
cd <top_dir>/iw/gtsrb/models/; source download_models.sh # download the model the first time
cd <top_dir>/build/finn
source iw_gtsrb; # to build gtsrb model
```
3) Generate xclbin file \
After you successfully generated xo files, use Vitis to link those xo files, produce xclbin and compile the host code  
```
cd <top_dir>/iw/gtsrb
cp <output_dir>/instrumentation/project_instrwrap/sol1/impl/export.xo sw
cp <output_dir>/xo/finn_design.xo sw
cd sw
make all TARGET=hw # this takes a few hours, to build xclbin
make run TARTGET=hw # if xclbin is successfully generated. This kicks off U250 run 
```



