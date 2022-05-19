# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG BASE_IMAGE=wngfra/dl-jupyterlab:base
FROM $BASE_IMAGE

# Install snntorch
RUN conda install --yes -c conda-forge brian2 && \
    conda install --yes -c brian-team brian2tools && \
    conda clean --all -f -y