# Installation

- you can use the compile commands such as:

```shell
# option 1. for cuda usage
./hpcc --install --target-feature "cuda8_0;cudnn"
# option 2. for cpu usage
./hpcc --install --config -DUSE_CUDA=OFF
# option 3. for fpga usage
./hpcc --install --target-arch armv7 --config -DUSE_CUDA=OFF -DCUSTOMIZED_OPENCV_PATH=${current_path}"/deps/opencv/armv7/" -DUSE_FPGA=ON -DUSE_PROTOBUF=OFF
```
- or the simpler commands:

```shell
# option 1. for cuda usage
sh compile.sh gpu
# option 2. for cpu usage
sh compile.sh cpu
# option 3. for fpga usage
sh compile.sh fpga
```