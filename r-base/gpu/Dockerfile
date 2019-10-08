FROM mcr.microsoft.com/azureml/base-gpu:latest

# R Packages
RUN conda install -c r -y r-essentials openssl=1.1.1c r-devtools && conda clean -ay

# AzureML SDK
# Get personal access token from https://github.com/settings/tokens
ARG GITHUB_AUTH_TOKEN
ENV TAR="/bin/tar"
RUN pip install --no-cache-dir azureml-defaults rpy2 && \
    R -e "devtools::install_github(repo = 'https://github.com/Azure/azureml-sdk-for-r', auth_token = '${GITHUB_AUTH_TOKEN}')"