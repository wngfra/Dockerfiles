# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=wngfra/ros2cuda:base
FROM $FROM_IMAGE

USER root

# Install SNN frameworks
RUN pip3 install -U brian2 brian2genn

WORKDIR /workspace
