# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=wngfra/ros2cuda:base
FROM $FROM_IMAGE

USER root

RUN mkdir -p /shared /opt

ENV LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

# Install nengo packages
RUN pip3 install -U nengo nengo-loihi
RUN mkdir -p /workspace/src

WORKDIR /workspace