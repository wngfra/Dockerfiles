# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=nvcr.io/nvidia/cuda:11.7.0-runtime-ubuntu22.04
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
RUN apt-get update && \
    apt-get install -q -y --no-install-recommends \
    build-essential \
    curl \
    git \
    gnupg \
    lsb-release \
    tar \
    tmux \
    vim \
    wget \
    xz-utils && \
    add-apt-repository universe

SHELL ["/bin/bash", "-c"]

# setup keys and sources.list
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Configure environment variables
ARG ROS_DISTRO=humble
ENV ROS_DISTRO=$ROS_DISTRO
ENV ROS_VERSION=2 \
    ROS_PYTHON_VERSION=3

# Install ros2 packages and other tools
RUN apt-get update && \
    apt-get install -q -y --no-install-recommends \
    ros-${ROS_DISTRO}-desktop \
    libeigen3-dev \
    libpoco-dev \
    python3-dev \
    python3-pip \
    libgl1-mesa-dev libx11-6 libxau6 libxcb1 libxkbcommon-x11-0 \
    xauth xvfb dbus-x11 x11-utils \
    libavcodec-dev libavformat-dev libswscale-dev \
    xsltproc && \
    apt-get autoclean -y && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 install -U\
    argcomplete \
    autopep8 \
    colcon-common-extensions \
    pyserial \
    pytest-rerunfailures

# Create a non-root user
RUN echo 'source "/opt/ros/$ROS_DISTRO/setup.bash"' >> /root/.bashrc \
    && echo 'export XLA_PYTHON_CLIENT_MEM_FRACTION=.60' >> /root/.bashrc

WORKDIR /workspace
