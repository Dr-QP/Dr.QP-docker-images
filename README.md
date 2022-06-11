[![Build Status](https://dev.azure.com/Dr-QP/Dr.QP/_apis/build/status/Dr-QP.Dr.QP-docker-images?branchName=master)](https://dev.azure.com/Dr-QP/Dr.QP/_build/latest?definitionId=1&branchName=master)

# Dr.QP-docker-images
All the docker images for Dr.QP robotics projects used for simulation, testing and deployment

## Base for raspi-64 bit

https://hub.docker.com/r/arm64v8/ros/tags?page=1&name=foxy

docker pull arm64v8/ros:foxy-ros-base

## Run with UART for motor control
DOCKER_HOST=tcp://192.168.1.136:2375 docker run --rm -it --device /dev/ttySC0 ros:foxy


### 

ros-foxy-ros-desktop

or
ros-foxy-demo-nodes-cpp ros-foxy-demo-nodes-cpp-native
  ros-foxy-demo-nodes-py
