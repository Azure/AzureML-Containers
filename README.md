# Azure Machine Learning base images
## Overview
This repository contains the release notes for the base images utilized in Azure Machine Learning.

## Purpose
These Docker images are employed as base images for training and inference within Azure ML. When submitting a training job on AmlCompute or any other Docker-enabled target, Azure ML executes your job in a Python environment within a Docker container.

## Publication and Patching
Base images are published to the Microsoft Container Registry (MCR) under the azureml/ namespace. AzureML provides security patches for supported images within 30 days of the availability of a fix. To ensure immutability, patches are not backported to previous versions of the images. Tags for these images can be found in the release notes or via the /tags/list API: https://mcr.microsoft.com/v2/azureml/openmpi4.1.0-ubuntu22.04/tags/list


## Compatibility and Support
AzureML does not guarantee backward compatibility. Images are provided “as-is” with a minimal set of installed packages to support most AzureML scenarios. AzureML images can be deprecated at any time without notice for security or compliance reasons. All released image versions remain available after the deprecation date, but no new versions will be released. Typically, images are supported until the corresponding Ubuntu end-of-life.

## Custom Containers
Users are not limited to using AzureML base images. Most AzureML scenarios support custom containers, allowing for greater flexibility and customization.
