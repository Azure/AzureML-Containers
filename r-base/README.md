# Azure Machine Learning R Containers
This folder contains Docker containers used in [Azure Machine Learning SDK for R](https://github.com/Azure/azureml-sdk-for-r).

## Table of Contents
- [Introduction](#introduction)
- [How Azure ML prepares an image for training](#howThingsWork)
- [Dependencies](#dependencies)
- [How to get Azure ML Docker Containers](#getdocker)
- [Featured Tags](#tags)
- [How to run an Azure ML experiment](#howtorun)
  * [Prerequisites](#prerequisites)
  * [Create Docker Image](#create)
  * [Build Docker Image](#build)
  * [Test Docker Image](#test)
  * [Push Docker Image](#push)
  * [Submit a training job](#submit)

<a name="introduction"></a>
### Introduction

These Docker images are used for training runs submitted via Azure ML. While submitting a training run on AmlCompute or any other target with Docker enabled, Azure ML runs your job in a conda environment within a Docker container with several dependencies installed.  You can also specify any extra dependencies to be installed using `cran_packages`, `github_packages`, and `custom_url_packages` parameters. The extra dependencies are installed on top of the dependencies in the Docker image.

<a name="howThingsWork"></a>
### How Azure ML prepares an image for training
If no `custom_docker_image` was specified in the [Estimator](https://azure.github.io/azureml-sdk-for-r/reference/estimator.html), Azure ML decides between a [CPU base image](./r-base/cpu) and a [GPU base image](./r-base/gpu) based on the `use_gpu` flag. The training happens on a Docker image built with all these dependencies. A new Docker image is built if this is the first time a combination of dependencies are used in a workspace. If not, a cached Docker image is used. The Docker image built for the training jobs are stored in an [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli) that is attached to your workspace. You can get the name of this ACR using `get_workspace_details(<your_workspace>)`. If there was a image build step, you can take a look at the logs to understand the steps involved to build the final Docker image.

<a name="dependencies"></a>
### Dependencies
The major dependencies installed in the base images are:

| Dependencies |   |
| --- | --- |
| r-devtools | 2.1.0 |
| r-essentials | 3.6.0 |
| rpy2 | 3.0.5 |
| openssl | 1.1.1c |

The both images are built on top of our [Azure ML CPU and GPU base images](./base/README.md), respectively.

<a name="getdocker"></a>
### How to get Azure ML Docker Containers

All images in this repository are published to [Microsoft Container Registry(MCR)](https://azure.microsoft.com/en-us/blog/microsoft-syndicates-container-catalog/). Information about these images are also published to [Docker Hub](https://hub.docker.com/_/microsoft-azureml).

You can pull these images from MCR using the following command.
- cpu image example: `docker pull mcr.microsoft.com\azureml\r-base:cpu`
- gpu image example: `docker pull mcr.microsoft.com\azureml\r-base:gpu`

If you observe the naming convention, image name and image tag information can be identified from the folder names in this repository.

GPU images pulled from MCR can only be used with Azure Services. Take a look at LICENSE.txt file inside the docker container for more information. GPU images are built from nvidia images. For NVIDIA CUDA and CUDNN take a look at the ThirdPartyNotices.txt file inside the docker container for more information about NVIDIA’s license terms

If you want to override the default image with another image from MCR or any publicly available image, you can do so by specifying an image in `custom_docker_image` parameter in the [Estimator](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#estimators).

<a name="tags"></a>
### Featured Tags

Each image is associated with one or more tags. Below is the list of images and associated tags.
- [CPU](./r-base/cpu)
    * r-base:cpu
    * r-base:latest
- [GPU](./r-base/gpu)
    * r-base:gpu

<a name="howtorun"></a>
###  How to run an Azure ML experiment using your own Dockerfile
You can use your own custom Docker container to submit a training job in Azure ML. Below are the steps to build your own container and use it in Azure ML training.

<a name="prerequisites"></a>
#### Prerequisites
 * Installation guide, quickstarts, end-to-end tutorials, and how-tos on the [official documentation site](https://azure.github.io/azureml-sdk-for-r/articles/).
 * [R SDK reference](https://azure.github.io/azureml-sdk-for-r/)

<a name="create"></a>
#### Create Docker image
Use Docker images in this repository to understand how Docker images are created for use in Azure ML.

<a name="build"></a>
#### Build Docker image
`docker build -f dockerfile -t image_name:tag path`
[Documentation for docker build](https://docs.docker.com/engine/reference/commandline/build/)

<a name="test"></a>
#### Test Docker image
Use `docker run` to test the image locally

<a name="push"></a>
#### Push Docker image
You can use images from [Docker Hub](https://hub.docker.com/) and [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli) in Azure ML. Push your image to the repository of your choice.

If needed by one of the training jobs you submitted in Azure ML, an Azure Container Registry is created on your behalf and attached to your [Azure ML workspace](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-workspaces). You can get this information using `get_workspace_details(<your_workspace>)`. If it was created, you can also get credentials of this ACR from Azure portal and reuse the same ACR to push your images.

<a name="submit"></a>
#### Submit a training job
Use `image_registry_details` and `custom_docker_image` parameters in the Estimator to use your own Dockerfile.
If your image is in a public repository, [here](https://azure.github.io/azureml-sdk-for-r/articles/train-with-tensorflow/train-with-tensorflow.html) is an example on how to use the image for training. If your image is in a private repository use `image_registry_details` parameter to specify the credentials for your repository.


This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
