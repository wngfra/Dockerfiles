# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG BASE_IMAGE=wngfra/dl-jupyterlab:base
FROM $BASE_IMAGE

# Install snntorch
RUN conda config --set channel_priority false && \
    conda install -c conda-forge --quiet --yes \
    brian2 \
    snntorch && \
    conda clean --all -f -y