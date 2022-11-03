# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=wngfra/ros2cuda:base
FROM $FROM_IMAGE

USER ubuntu

ENV LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

# Install pyhton packages
RUN pip3 install -U mujoco dm-control nengo scikit-learn moderngl
RUN mkdir -p /home/ubuntu/workspace/src

WORKDIR /home/ubuntu/workspace