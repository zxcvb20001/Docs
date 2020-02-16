# About
This is a project developed by sensedrive team.

adas-pro project runs on only one model for lane detection, vehicle detection, pedestrian detection and cyclist detection. Freespace, lane type and traffic light/sign will be added very soon.

## Table of Contents

- [Features](#features)
- [Dependences](#dependences)
- [Installation](#installation)
- [Development](#development)
- [Modules](#modules)
- [How-to-run](#how-to-run)
  - [Visualizer](#visualizer)
  - [Roadtest](#roadtest)
  - [Evaluation](#evaluation)
  - [Sample](#sample)
- [See Also](#see-also)

## Features

| Features                  | Description               |
| :------------------------ | :-------------------      |
| Lane lines detection      | Lane line detection       |
| Lane departure warning    | Warning decison is based on the distance between car and lane line  |
| Objects detection         | Vehicle, pedestrian, cyclist and traffic sign/light detection |
| Front collision warning   | Warning decision is based on the distance and the relative speed |
| freespace detection       | Drivable region detection |

## Dependences

- System dependences
  - **cmake:** an open-source, cross-platform family of tools designed to build, test and package software. ***version 3.0 and above is required.***
  - **opencv:** Open Source Computer Vision Library. ***version 2.4.13 is required and located in `/usr/local/opencv-2.4.13`***

- Gpu version specific dependences:
  - **cuda:** a parallel computing platform and programming model developed by NVIDIA for general computing on graphical processing units. ***version 8.0 is required.***
  - **cudnn:** Deep Neural Network library is a GPU-accelerated library of primitives for deep neural networks. ***version 5.1 for aarch64 and version 5.0 for x86_64***

- Project dependences

  - **gtest:** http://gitlab.sz.sensetime.com/linan/gtest, only used in unittest
  - **cereal:** https://github.com/USCiLab/cereal , only used in input and output serialization
  - **cppzmq:** https://github.com/zeromq/cppzmq , only used in roadtest communication
  - **libzmq:** https://github.com/zeromq/libzmq , only used in roadtest communication
  - **alglib:** http://gitlab.hk.sensetime.com/SenseDrive/alglib
  - **protobuf:** http://gitlab.sz.sensetime.com/linan/protobuf
  - **glog:** https://github.com/google/glog.git
  - **gflags:** https://github.com/gflags/gflags.git
  - **post_release_lib:** http://gitlab.sz.sensetime.com/lijiabin/post_release_lib
  - **sdk_protector:** http://gitlab.hk.sensetime.com/SenseDrive/sdk_protector
  - **sensedrive-core:** http://gitlab.hk.sensetime.com/SenseDrive/sensedrive-core
  - **sensedrive-common:** http://gitlab.hk.sensetime.com/SenseDrive/sensedrive-common
  - **object-detection:** http://gitlab.hk.sensetime.com/SenseDrive/object-detection
  - **CNNiE-SoC:** http://gitlab.sz.sensetime.com/maoningyuan/CNNiE-SoC.git, a fpga framework library

## Installation
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

## Development
- compile

```shell
# step 1. config git lfs
git config --global lfs.url "http://devcenter.bj.sensetime.com/api/v1/lfs/"
git config --global credential.helper store
# step 2. clone the project and switch to Develop branch
git clone git@gitlab.sz.sensetime.com:SenseDrive-China/adas.git
git checkout Develop
# step 3. make sure that you can compile the code with no errors
sh compile.sh fpga
# step 4. describe your feature with an issue and checkout new branch
git checkout -b ADASALPHA-ISSUEID_your_feature
# step 5. cpplint checkout your code
cpplint --recursive module_you_modified
# step 6. make sure your git is clean
git add .
git commit -m "ADASALPHA-ISSUEID your commit message"
git rebase HEAD~2
# step 7. make sure your git can be merged directly
git checkout Develop
git pull origin Develop
git checkout ADASALPHA-ISSUEID_your_feature
git rebase Develop
# step 8. send a real merge request
git push origin ADASALPHA-ISSUEID_your_feature -f
```

## Modules
- **framework**: framework contains the SDK part. no thirdparty library was used in SDK src code.
- **c-api**: c-api is a wrapper of SDK input and output. no SDK prototype was exported to user.
- **roadtest**: roadtest contains a solution for ADAS road test. It can transfer the results and images to the laptop through the ethernet, then the laptop will show the ADAS results on screen and save the orginal video and result video.
- **visualizer**: visualizer is used to run the algorithm and output the results such as lane lines, vehicle, pedestrian, cyclist and traffic sign.
- **evaluation**: evaluation is used to evaluate the detection models.With the images and the corespond groundtruth, it will output some algorithm index such as precision, accuracy, recall, mAP, mIou and so on.

## How-to-run

### Visualizer

- gpu version

```shell
cd target/linux-x86_64_cuda8_0_cudnn5_1/Release/bin/visualizer/
# change the config file, such as model file, input images path, output images path
vi config_files/config.json
./visualizer config_files/config.json
```

- fpga version

```shell
cd target/linux-armv7/Release/bin/
scp -r ./visualizer root@172.20.7.209:/mnt/xxx
ssh root@172.20.7.209
cd /mnt/xxx
# change the config file, such as model file, input images path, output images path
vi config_files/config.json
./visualizer config_files/config.json
```

### Roadtest

- embedded board

```shell
cd target/linux-armv7/Release/bin/
scp -r ./roadtest root@172.20.7.209:/mnt/xxx
ssh root@172.20.7.209
cd /mnt/xxx
# change the config file, such as model file, input images path, output images path
vi config_files/config.json
./roadtest_core config_files/config.json
```

- laptop

```shell
cd target/linux-x86_64/Release/bin/roadtest
# change the config file, such as the sender's ip and output video path
vi config_files/config.json
# make sure you mkdir the output video directory
mkdir original_videos && mkdir videos
./roadtest_preview config_files/config.json
```

### Evaluation

- Generate the image list file

```shell
cd tool/evaluation/scripts
# change the test dateset root_path and the generated_list_file's saved_path
vi generate_image_list_lane.py
python generate_image_list_lane.py
```

- Run the evaluation

``` shell
cd target/linux-x86_64/Release/bin/evaluation
# change the config file, such as the model path, input image size, dataset root path and dataset image list path
vi config_files/config.json
./evaluation config_files/config.json
```

### Sample

``` shell
cd install/adas
mkdir build
cmake ..
./adas_detection_sample config_yml.example
```

## See Also
Test Report shows some models speed test results.
