# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=ubuntu:latest
FROM $FROM_IMAGE

# setup timezone
RUN apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata \
    locales \
    software-properties-common && \
    echo 'Etc/UTC' > /etc/timezone && \
    ln -sf /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    locale-gen en_US en_US.UTF-8 && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    build-essential \
    curl \
    git \
    gnupg \
    lsb-release && \
    add-apt-repository universe && \
    apt autoclean -y

# setup keys and sources.list
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Configure environment variables
ARG ROS_DISTRO=humble
ENV ROS_DISTRO=$ROS_DISTRO
ENV ROS_VERSION=2 \
    ROS_PYTHON_VERSION=3

# install ros2 packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-${ROS_DISTRO}-desktop \
    libeigen3-dev \
    libpoco-dev \
    python3-dev \
    python3-pip \ 
    python3-vcstool && \
    rm -rf /var/lib/apt/lists/*

# Download, build and install libfranka
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
WORKDIR /ubuntu/ros2_ws
USER ubuntu

# Get Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y && \
    echo 'source $HOME/.cargo/env' >> /ubuntu/.bashrc

# Add ROS2 python tools
RUN pip3 install -U\
    argcomplete \
    colcon-common-extensions \
    pytest-rerunfailures

# Install these plugins for cargo and colcon:
SHELL ["/bin/bash", "-c", "source /ubuntu/.bashrc"]
RUN cargo install --debug cargo-ament-build  && \
    pip install git+https://github.com/colcon/colcon-cargo.git && \
    pip install git+https://github.com/colcon/colcon-ros-cargo.git

RUN mkdir -p /ubuntu/ros2_ws/src && \
    git clone https://github.com/ros2-rust/ros2_rust.git src/ros2_rust && \
    vcs import src < src/ros2_rust/ros2_${ROS_DISTRO}.repos && \
    . /opt/ros/foxy/setup.sh && \
    colcon build

CMD ["bash"]