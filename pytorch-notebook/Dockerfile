# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG BASE_IMAGE=nvcr.io/nvidia/pytorch:21.12-py3
FROM $BASE_IMAGE

# Install Jupyter Lab, and Hub
# Generate a notebook server config
# Cleanup temporary files
# Correct permissions
# Do all this in a single RUN command to avoid duplicating all of the
# files across image layers when the permissions change
RUN conda update conda --yes && \
    conda install -c conda-forge --quiet --yes \
    nodejs \
    jupyterhub \
    jupyterlab \
    jupyterthemes \
    matplotlib \
    numpy \
    scipy \
    seaborn && \
    npm cache clean --force && \
    jupyter lab clean && \
    conda clean --all -f -y

RUN pip3 install --upgrade --no-cache-dir \
    bidict \
    ipympl \
    scikit-fda \
    tensorly

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

EXPOSE 8888
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["jupyter-lab", "--port=8888", "--no-browser", "--allow-root"]