# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=wngfra/ros2cuda:base
FROM $FROM_IMAGE

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    vim tar xz-utils wget \
    libx11-6 libxcb1 libxau6 libgl1-mesa-dev \
    xvfb dbus-x11 x11-utils libxkbcommon-x11-0 \
    libavcodec-dev libavformat-dev libswscale-dev \
    && \
    apt-get autoclean -y && apt-get autoremove -y && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /shared /opt

ARG FILENAME=CoppeliaSim_Edu_V4_3_0_rev12_Ubuntu20_04

RUN wget https://www.coppeliarobotics.com/files/${FILENAME}.tar.xz /opt/

RUN tar -xf /opt/${FILENAME}.tar.xz  -C /opt && \
    rm /opt/${FILENAME}.tar.xz 

ENV COPPELIASIM_ROOT_DIR=/opt/${FILENAME}
ENV LD_LIBRARY_PATH=$COPPELIASIM_ROOT_DIR:$LD_LIBRARY_PATH
ENV PATH=$COPPELIASIM_ROOT_DIR:$PATH

WORKDIR /workspace
CMD ["bash"]