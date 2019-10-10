FROM mcr.microsoft.com/azureml/base:latest

# R Packages
RUN conda install -c r -y r-essentials=3.6.0 openssl=1.1.1c && conda install -c conda-forge r-devtools=2.0.1 && conda clean -ay

# AzureML SDK
ENV TAR="/bin/tar"
RUN pip install --no-cache-dir azureml-defaults rpy2==3.0.5 && \
    R -e "devtools::install_github(repo = 'https://github.com/Azure/azureml-sdk-for-r')"