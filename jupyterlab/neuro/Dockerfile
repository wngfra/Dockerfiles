# Copyright (c) 2022 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG BASE_IMAGE=wngfra/jupyterlab:base
FROM $BASE_IMAGE

# Install snntorch
RUN conda install --yes -c pytorch -c conda-forge \
    brian2 \
    nengo \
    nengo-gui \
    opencv \
    pytorch torchvision torchaudio cudatoolkit=11.6 && \
    conda clean --all -f -y
    

RUN git clone https://github.com/brian-team/brian2tools.git ~/brian2tools && \
    pip install -e ~/brian2tools/ && \
    pip install -U alive_progress mujoco pytorch-spiking

WORKDIR /workspace