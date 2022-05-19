# Copyright (c) 2020 wngfra
# Use of this source code is governed by the Apache-2.0 license, see LICENSE

ARG FROM_IMAGE=nvidia/opengl:base
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
    git \
    libqt5x11extras5 \
    python3 \
    python3-dev \
    python3-pip \
    python3-opengl && \
    apt autoclean -y

# install python packages
RUN pip3 install -U\
    gym \
    tacto \
    pybulletx

# clone tacto repo
RUN git clone https://github.com/facebookresearch/tacto.git /tacto
WORKDIR /tacto

CMD ["bash"]
