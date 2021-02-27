# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

FROM mcr.microsoft.com/azureml/o16n-base/python-assets:20210210.31228572 AS inferencing-assets

# Tag: cuda:11.0.3-devel-ubuntu18.04
# Env: CUDA_VERSION=11.0.3
# Env: NCCL_VERSION=2.8.3
# Env: CUDNN_VERSION=8.0.5.39

FROM nvidia/cuda:11.0.3-cudnn8-devel-ubuntu18.04

USER root:root

ENV com.nvidia.cuda.version $CUDA_VERSION
ENV com.nvidia.volumes.needed nvidia_driver
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64
ENV NCCL_DEBUG=INFO
ENV HOROVOD_GPU_ALLREDUCE=NCCL

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
    libbz2-1.0 \
    systemd \
    git \
    wget \
    cpio \
    pciutils \
    libnuma-dev \
    ibutils \
    ibverbs-utils \ 
    rdmacm-utils \
    infiniband-diags \
    perftest \
    librdmacm-dev \
    libibverbs-dev \
    libsm6 \
    libxext6 \
    libxrender-dev \
    libssl1.0.0 \
    linux-image-aws \ 
    linux-image-azure \ 
    linux-image-generic \
    linux-image-kvm \
    linux-image-lowlatency \
    linux-image-virtual \
    linux-image-gke \
    linux-image-oem \
    slapd \
    perl \ 
    ca-certificates \
    apt \
    p11-kit \
    libp11-kit0 \
    tar \
    fuse && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Inference
# Copy logging utilities, nginx and rsyslog configuration files, IOT server binary, etc.
COPY --from=inferencing-assets /artifacts /var/
RUN /var/requirements/install_system_requirements.sh && \
    cp /var/configuration/rsyslog.conf /etc/rsyslog.conf && \
    cp /var/configuration/nginx.conf /etc/nginx/sites-available/app && \
    ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/app && \
    rm -f /etc/nginx/sites-enabled/default
ENV SVDIR=/var/runit
ENV WORKER_TIMEOUT=300
EXPOSE 5001 8883 8888

# Conda Environment
ENV MINICONDA_VERSION py37_4.9.2
ENV PATH /opt/miniconda/bin:$PATH
RUN wget -qO /tmp/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf

# Open-MPI-UCX installation
RUN mkdir /tmp/ucx && \
    cd /tmp/ucx && \
	wget -q https://github.com/openucx/ucx/releases/download/v1.6.1-rc2/ucx-1.6.1.tar.gz && \
	tar zxf ucx-1.6.1.tar.gz && \
	cd ucx-1.6.1 && \
	./configure --prefix=/usr/local --enable-optimizations --disable-assertions --disable-params-check --enable-mt && \
	make -j $(nproc --all) && \
	make install && \
	rm -rf /tmp/ucx

# Open-MPI installation
ENV OPENMPI_VERSION 4.1.0
RUN mkdir /tmp/openmpi && \
    cd /tmp/openmpi && \
    wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-${OPENMPI_VERSION}.tar.gz && \
    tar zxf openmpi-${OPENMPI_VERSION}.tar.gz && \
    cd openmpi-${OPENMPI_VERSION} && \
    ./configure --with-ucx=/usr/local/ --enable-mca-no-build=btl-uct --enable-orterun-prefix-by-default && \
    make -j $(nproc) all && \
    make install && \
    ldconfig && \
    rm -rf /tmp/openmpi	
	
# Msodbcsql17 installation
RUN apt-get update && \
    apt-get install -y curl && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql17

#Cmake Installation
RUN apt-get update && \
    apt-get install -y cmake
