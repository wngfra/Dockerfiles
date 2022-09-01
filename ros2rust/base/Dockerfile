# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=osrf/ros:humble-desktop-full
FROM $FROM_IMAGE

# Install packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    clang \
    libclang-dev \
    libeigen3-dev \
    libpoco-dev \
    python3-pip \
    python3-vcstool \
    tmux && \
    rm -rf /var/lib/apt/lists/*

# Build libfranka
RUN git clone --recursive https://github.com/frankaemika/libfranka.git /usr/src/libfranka
WORKDIR /usr/src/libfranka/build
RUN cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF .. \
    && make \
    && make install

RUN pip install --upgrade pytest 

# Get Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | bash -s -- -y
ENV PATH=/root/.cargo/bin:$PATH
ENV CXX=clang++

# Install these plugins for cargo and colcon:
RUN cargo install --debug cargo-ament-build  && \
    pip install git+https://github.com/colcon/colcon-cargo.git && \
    pip install git+https://github.com/colcon/colcon-ros-cargo.git

RUN mkdir -p /workspace && echo "Did you forget to mount the repository into the Docker container?" > /workspace/HELLO.txt
WORKDIR /workspace