# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=osrf/ros:humble-desktop-full
FROM $FROM_IMAGE

# Install packages
RUN apt-get update && apt-get install -y --no-install-recommends \
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

# Create a non-root user
RUN useradd -rm -d /ubuntu -s /bin/bash -u 1000 ubuntu \
    && echo 'source "/opt/ros/$ROS_DISTRO/setup.bash"' >> /ubuntu/.bashrc \
    && echo 'export XLA_PYTHON_CLIENT_MEM_FRACTION=.60' >> /ubuntu/.bashrc

# Change default user
USER ubuntu

# Add ROS2 python tools
RUN pip install -U\
    argcomplete \
    colcon-common-extensions \
    pytest-rerunfailures

# Get Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | bash -s -- -y && \
    echo 'source $HOME/.cargo/env' >> /ubuntu/.bashrc
SHELL ["/bin/bash", "-c", "source /ubuntu/.bashrc"]

# Install these plugins for cargo and colcon:
RUN cargo install --debug cargo-ament-build  && \
    pip install git+https://github.com/colcon/colcon-cargo.git && \
    pip install git+https://github.com/colcon/colcon-ros-cargo.git

WORKDIR /ubuntu

CMD ["bash"]