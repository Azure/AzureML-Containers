FROM ubuntu:16.04

USER root:root

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive

# Base conda installation
ENV MINICONDA_VERSION 4.7.12.1
ENV PATH /opt/miniconda/bin:$PATH
RUN apt-get update -y && \
    apt-get install -y \
        bzip2=1.0.6-8ubuntu0.2 \
        wget \
        && \
    wget -qO /tmp/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Copy TPN & License files
COPY ThirdPartyNotices.txt LICENSE.txt VERSION /


