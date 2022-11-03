# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=wngfra/ros2cuda:base
FROM $FROM_IMAGE

USER root

RUN echo 'source "/opt/ros/$ROS_DISTRO/setup.bash"' >> /root/.bashrc \
    && echo 'export XLA_PYTHON_CLIENT_MEM_FRACTION=.60' >> /root/.bashrc

RUN mkdir -p /shared /opt

ARG FILENAME=CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu20_04

RUN wget https://www.coppeliarobotics.com/files/${FILENAME}.tar.xz -P /opt/

RUN tar -xf /opt/${FILENAME}.tar.xz -C /opt/ && \
    rm /opt/${FILENAME}.tar.xz

ENV COPPELIASIM_ROOT_DIR=/opt/${FILENAME}
ENV LD_LIBRARY_PATH=$COPPELIASIM_ROOT_DIR:$LD_LIBRARY_PATH
ENV PATH=$COPPELIASIM_ROOT_DIR:$PATH

# build Coppeliasim ROS2 plugin
RUN pip3 install -U cbor pyzmq xmlschema 
RUN mkdir -p /workspace/src && \
    cp -r ${COPPELIASIM_ROOT_DIR}/programming/ros2_packages /workspace/src/

WORKDIR /workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash && \
    ulimit -s unlimited && \
    colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
