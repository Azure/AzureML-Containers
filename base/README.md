# Azure Machine Learning service containers
This repository contains Docker containers used in [Azure Machine Learning for Python](https://azure.microsoft.com/en-us/services/machine-learning-service/) Python SDK.

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

These Docker images are used for training runs submitted via Azure ML. While submitting a training run on AmlCompute or any other target with Docker enabled, Azure ML runs your job in a conda environment within a Docker container with several dependencies installed.  You can also specify any extra dependencies to be installed using `pip_packages`, `pip_requirements_file_path`, `conda_dependencies_file` and `conda_packages` parameters. The extra dependencies are installed on top of the dependencies in the Docker image. If you are using the estimators specific for [DNN training](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py), the DNN framework related dependencies are also installed. You can control the version of the DNN installed using the `framework_version` parameter. If no version is specified, default version will be used.

<a name="howThingsWork"></a>
### How Azure ML prepares an image for training
For example, if you are using the [PyTorch Estimator](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), these are steps in the background.
1. If no custom_docker_image was specified, Azure ML decides between [CPU base images](./base/cpu) and [GPU base images](./base/gpu) based on the `use_gpu` flag.
2. Based on the `framework_version` Azure ML selects the list of framework based dependencies to be added. If not specified, the default framework version will be used.
3. For the PyTorch estimator the framework specific dependencies are torch==1.0, torchvision==0.2.1 and horovod==0.15.2
4. The training happens on a Docker image built with all these dependencies. A new Docker image is built if this is the first time a combination of dependencies are used in a workspace. If not, a cached Docker image is used. The Docker image built for the training jobs are stored in an [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli) that is attached to your workspace. You can get the name of this ACR using `workspace.get_details()`. If there was a image build step, you can take a look at the logs to understand the steps involved to build the final Docker image.

<a name="dependencies"></a>
### Dependencies
Currently Azure ML supports both cuda9 and cuda10 base images. The major dependencies installed in the base images are:

| Dependencies | IntelMPI CPU | OpenMPI CPU | IntelMPI GPU | OpenMPI GPU |
| --- | --- | --- | --- | --- |
| miniconda | ==4.5.11 | ==4.5.11 | ==4.5.11 | ==4.5.11 |
| mpi | intelmpi==2018.3.222 |openmpi==3.1.2 |intelmpi==2018.3.222| openmpi==3.1.2 |
| cuda | - | - | 9.0/10.0 | 9.0/10.0 |
| cudnn | - | - | 7.4/7.5 | 7.4/7.5 |
| nccl | - | - | 2.4 | 2.4 |
| git | 2.7.4 | 2.7.4 | 2.7.4 | 2.7.4 |

The CPU images are built from ubuntu16.04. The GPU images for cuda9 are built from nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04. The GPU images for cuda10 are built from nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04.

<a name="getdocker"></a>
### How to get Azure ML Docker Containers

All images in this repository are published to [Microsoft Container Registry(MCR)](https://azure.microsoft.com/en-us/blog/microsoft-syndicates-container-catalog/). Information about these images are also published to [Docker Hub](https://hub.docker.com/_/microsoft-azureml).

You can pull these images from MCR using the following command.
- cpu image example: `docker pull mcr.microsoft.com\azureml\base:intelmpi2018.3-ubuntu16.04`
- gpu image example: `docker pull mcr.microsoft.com\azureml\base-gpu:intelmpi2018.3-ubuntu16.04`

If you observe the naming convention, image name and image tag information can be identified from the folder names in this repository.

GPU images pulled from MCR can only be used with Azure Services. Take a look at LICENSE.txt file inside the docker container for more information. GPU images are built from nvidia images. For NVIDIA CUDA and CUDNN take a look at the ThirdPartyNotices.txt file inside the docker container for more information about NVIDIA’s license terms

Currently Azure ML is using [IntelMPI CPU](./base/cpu/intelmpi2018.3-ubuntu16.04) as the default base image for training on a CPU compute target and [IntelMPI GPU](./base/gpu/intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04) as the default base image for training on a GPU compute target. If you want to override the default image with another image from MCR or any publicly available image, you can do so by specifying an image in `custom_docker_image` parameter in the [Azure ML Estimators](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py). You can specify a `custom_docker_image` parameter in both Generic Estimators and any DNN Estimators provided by Azure ML.

<a name="tags"></a>
### Featured Tags

Each image is associated with one or more tags. Below is the list of images and associated tags.
- [IntelMPI CPU](./base/cpu/intelmpi2018.3-ubuntu16.04)
    * base:intelmpi2018.3-ubuntu16.04
    * base:latest
- [OpenMPI CPU](./base/cpu/openmpi3.1.2-ubuntu16.04)
    * base:openmpi3.1.2-ubuntu16.04
- [IntelMPI GPU - cuda9](./base/gpu/intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04)
    * base-gpu:intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04
- [OpenMPI GPU - cuda9](./base/gpu/openmpi3.1.2-cuda9.0-cudnn7-ubuntu16.04)
    * base-gpu:openmpi3.1.2-cuda9.0-cudnn7-ubuntu16.04
- [IntelMPI GPU - cuda10](./base/gpu/intelmpi2018.3-cuda10.0-cudnn7-ubuntu16.04)
    * base-gpu:intelmpi2018.3-cuda10.0-cudnn7-ubuntu16.04
    * base-gpu:latest
- [OpenMPI GPU - cuda10](./base/gpu/openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04)
    * base-gpu:openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04

<a name="howtorun"></a>
###  How to run an Azure ML experiment using your own Dockerfile
You can use your own custom Docker container to submit a training job in Azure ML. Below are the steps to build your own container and use it in Azure ML training.

<a name="prerequisites"></a>
#### Prerequisites
 * Install Azure ML SDK for Python and [setup environment](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-run-local-notebook)
 * Quickstarts, end-to-end tutorials, and how-tos on the [official documentation site for Azure Machine Learning service](https://docs.microsoft.com/en-us/azure/machine-learning/service/).
 * [Python SDK reference](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/intro?view=azure-ml-py)

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

If needed by one of the training jobs you submitted in Azure ML, an Azure Container Registry is created on your behalf and attached to your [Azure ML workspace](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace). You can get this information using `workspace.get_details()`. If it was created, you can also get credentials of this ACR from Azure portal and reuse the same ACR to push your images.

<a name="submit"></a>
#### Submit a training job
Use `image_registry_details` and `custom_docker_image` parameters in Azure ML Estimators to use your own Dockerfile.
If your image is in a public repository, [here](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-cntk-with-custom-docker/distributed-cntk-with-custom-docker.ipynb) is an example on how to use the image for training. If your image is in a private repository use `image_registry_details` parameter to specify the credentials for your repository.

## Projects using Azure Machine Learning

Visit following repositories to see the projects contributed by Azure ML users:

 - [Fine tune natural language processing models using Azure Machine Learning service](https://github.com/Microsoft/AzureML-BERT)
 - [Fashion MNIST with Azure ML SDK](https://github.com/amynic/azureml-sdk-fashion)


This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
