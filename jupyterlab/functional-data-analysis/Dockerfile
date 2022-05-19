# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG BASE_IMAGE=wngfra/dl-jupyterlab:base
FROM $BASE_IMAGE

# Install Jupyter Lab, and Hub
# Generate a notebook server config
# Cleanup temporary files
# Correct permissions
# Do all this in a single RUN command to avoid duplicating all of the
# files across image layers when the permissions change
RUN conda install --yes pandas && \
    conda clean --all -f -y

RUN pip3 install --upgrade --no-cache-dir \
    bidict \
    ipympl \
    scikit-fda