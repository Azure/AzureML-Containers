# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

FROM ubuntu:16.04

USER root:root

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive

# Install Common Dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    # SSH and RDMA
    libmlx4-1 \
    libmlx5-1 \
    librdmacm1 \
    libibverbs1 \
    libmthca1 \
    libdapl2 \
    dapl2-utils \
    openssh-client \
    openssh-server \
    iproute2 && \
    # Others
    apt-get install -y \
    build-essential \
    bzip2 \
    wget \
    cpio && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Conda Environment
ENV MINICONDA_VERSION 4.5.11
ENV PATH /opt/miniconda/bin:$PATH
RUN wget -qO /tmp/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf

# Open-MPI installation
ENV OPENMPI_VERSION 3.1.2
RUN mkdir /tmp/openmpi && \
    cd /tmp/openmpi && \
    wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-${OPENMPI_VERSION}.tar.gz && \
    tar zxf openmpi-${OPENMPI_VERSION}.tar.gz && \
    cd openmpi-${OPENMPI_VERSION} && \
    ./configure --enable-orterun-prefix-by-default && \
    make -j $(nproc) all && \
    make install && \
    ldconfig && \
    rm -rf /tmp/openmpi
