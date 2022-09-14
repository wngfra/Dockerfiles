# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=osrf/ros:humble-desktop-full
FROM $FROM_IMAGE

SHELL ["/bin/bash", "-c"]

# Install packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    clang \
    libclang-dev \
    software-properties-common \
    python3-pip \
    python3-vcstool \
    tmux \
    vim

ENV CXX=clang++

RUN add-apt-repository ppa:nest-simulator/nest && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    nest && \
    rm -rf /var/lib/apt/lists/*

RUN pip install -U autopep8 matplotlib nengo numpy pyserial scipy

RUN mkdir -p /workspace && echo "Did you forget to mount the repository into the Docker container?" > /workspace/HELLO.txt
WORKDIR /workspace
ENTRYPOINT /ros_entrypoint.sh