# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

FROM mcr.microsoft.com/azureml/o16n-base/python-assets@sha256:ea4c587a6f054cef0c530b293eb4320a6b8d0e03ee982c5884562cecbf64ee19 AS inferencing-assets

FROM nvcr.io/nvidia/tritonserver:20.08-py3

USER root:root

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive

# Install Common Dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    # SSH and RDMA
    libmlx5-1 \
    librdmacm1 \
    libibverbs1 \
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
    libxrender-dev \
    fuse \
    libcurl3-gnutls \
    libcurl4 \
    libcurl4-openssl-dev && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Inference
# Copy logging utilities and rsyslog configuration files, etc.
COPY --from=inferencing-assets /artifacts /var/
RUN /var/requirements/install_system_requirements.sh && \
    cp /var/configuration/rsyslog.conf /etc/rsyslog.conf
RUN sed -i 's/127.0.0.1:31311/0.0.0.0:5001/' /var/azureml-server/gunicorn_conf.py
ENV WORKER_TIMEOUT=300
EXPOSE 5001 8883 8888

# Conda Environment
ENV MINICONDA_VERSION 4.7.12.1
ENV PATH /opt/miniconda/bin:$PATH
RUN wget -qO /tmp/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf

# Install curl
RUN apt-get update && \
    apt-get install -y curl

# remove extra part
RUN rm -rf /var/iot-server && \
    rm -rf /var/runit/iot-server && \
    rm -rf /var/runit/nginx

# Setup trtion inference server
COPY --chown=root:root --from=inferencing-assets /artifacts/triton_runit/gunicorn /var/runit/gunicorn
COPY --chown=root:root --from=inferencing-assets /artifacts/triton_runit/triton /var/runit/triton
RUN chmod +x -R /var/runit/gunicorn/*
RUN chmod +x -R /var/runit/triton/*
ENV OMP_WAIT_POLICY=PASSIVE
ENV WORKER_COUNT=64
EXPOSE 8000 8001 8002

ENTRYPOINT []
