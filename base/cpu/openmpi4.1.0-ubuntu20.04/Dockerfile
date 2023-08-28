# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.
FROM mcr.microsoft.com/azureml/o16n-base/python-assets:20230222.v4 AS inferencing-assets

FROM ubuntu:20.04

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
    libbz2-1.0 \
    systemd \
    git \
    wget \
    cpio \
    libsm6 \
    libxext6 \
    libssl1.1 \
    libxrender-dev \
    libx11-dev \
    libcurl3-nss \
    libgd3 \
    binutils-multiarch \
    libxml2-dev \ 
    #adding package to mitigate vulnerabilities.
    e2fsprogs \
    e2fsck-static \
    fuse2fs \
    libgnutls30 \
    libpcre2-8-0 \
    libgmp10 \
    zlib1g \
    uidmap \
    libkrb5-26-heimdal \     
    libhcrypto4-heimdal \    
    libheimntlm0-heimdal \    
    libheimbase1-heimdal \    
    libasn1-8-heimdal \    
    libgssapi3-heimdal \    
    libhx509-5-heimdal \    
    libroken18-heimdal \    
    libwind0-heimdal \
    libtinfo6 \
    libncurses6 \ 
    ncurses-bin \
    ncurses-base \
    libncursesw6 \
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
ENV MINICONDA_VERSION py38_23.3.1-0
ENV PATH /opt/miniconda/bin:$PATH
ENV CONDA_PACKAGE 23.5.0
RUN wget -qO /tmp/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    conda install conda=${CONDA_PACKAGE} -y && \
    conda install wheel=0.38.1 setuptools=65.5.1 cryptography=41.0.3 requests=2.31.0 -c conda-forge -y && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf  

# Open-MPI installation
ENV OPENMPI_VERSION 4.1.0
RUN mkdir /tmp/openmpi && \
    cd /tmp/openmpi && \
    wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-${OPENMPI_VERSION}.tar.gz && \
    tar zxf openmpi-${OPENMPI_VERSION}.tar.gz && \
    cd openmpi-${OPENMPI_VERSION} && \
    ./configure --enable-orterun-prefix-by-default && \
    make -j $(nproc) all && \
    make install && \
    ldconfig && \
    rm -rf /tmp/openmpi

# Msodbcsql17 installation
RUN apt-get update && \
    apt-get install -y curl && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql17
