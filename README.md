# Azure Machine Learning base images
This repository contains Dockerfiles for the base images used in [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning-service/).

### Table of Contents
- [Introduction](#introduction)
- [Base image dependencies](#dependencies)
- [How to get Azure ML images](#getdocker)
  * [Featured tags](#tags)
- [Using Azure ML base images for training](#howtorun)
  * [Prerequisites](#prerequisites)
  * [Create Azure ML Environment](#createenv)
  * [Configure and submit training job](#submitjob)
  * [What happens during job execution](#jobexe)
- [Using your own custom Docker image or Dockerfile for training](#customdocker)
- [Resources](#resources)

<a name="introduction"></a>
## Introduction

These Docker images serve as base images for training and inference in Azure ML. While submitting a training job on AmlCompute or any other target with Docker enabled, Azure ML runs your job in a conda environment within a Docker container.

You can also use these Docker images as base images for your custom Azure ML [Environments](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py). If you specify any conda dependencies in your Environment, the extra dependencies are installed on top of the dependencies in the Docker image.

**Note that these base images do not come with Python packages, notably the Azure ML Python SDK, installed.** If you require the Azure ML SDK package for your job, make sure you also install the appropriate package.

<a name="dependencies"></a>
## Base image dependencies
Currently Azure ML supports both cuda9 and cuda10 base images. The major dependencies installed in the base images are:

| Dependencies | IntelMPI CPU | OpenMPI CPU | IntelMPI GPU | OpenMPI GPU |
| --- | --- | --- | --- | --- |
| miniconda | ==4.5.11 | ==4.5.11 | ==4.5.11 | ==4.5.11 |
| mpi | intelmpi==2018.3.222 |openmpi==3.1.2 |intelmpi==2018.3.222| openmpi==3.1.2 |
| cuda | - | - | 9.0/10.0 | 9.0/10.0/10.1/10.2 |                              
| cudnn | - | - | 7.4/7.5 | 7.4/7.5/7.6/8.0 |               
| nccl | - | - | 2.4 | 2.4/2.4/2.4/2.7 |
| git | 2.7.4 | 2.7.4 | 2.7.4 | 2.7.4 |

The CPU images are built from ubuntu16.04/ubuntu18.04. The GPU images for cuda9 are built from nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04. The GPU images for cuda10 are built from:
* nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04
* nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04
* nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
* nvidia/cuda:10.2-cudnn8-devel-ubuntu18.04

<a name="getdocker"></a>
## How to get the Azure ML images

All images in this repository are published to [Microsoft Container Registry (MCR)](https://azure.microsoft.com/en-us/blog/microsoft-syndicates-container-catalog/).

You can pull these images from MCR using the following command:
- CPU image example: `docker pull mcr.microsoft.com/azureml/openmpi3.1.2-ubuntu18.04`
- GPU image example: `docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04`

If you observe the naming convention, image tag maps to the folder name that contains the corresponding Dockerfile.

GPU images pulled from MCR can only be used with Azure Services. Take a look at LICENSE.txt file inside the docker container for more information. GPU images are built from nvidia images. For NVIDIA CUDA and cuDNN take a look at the ThirdPartyNotices.txt file inside the docker container for more information about NVIDIA’s license terms

<a name="tags"></a>
### Featured tags
Below is the list of tags:
- [IntelMPI CPU - Ubuntu 16.04](./base/cpu/intelmpi2018.3-ubuntu16.04)
    * intelmpi2018.3-ubuntu16.04
- [OpenMPI CPU - Ubuntu 16.04](./base/cpu/openmpi3.1.2-ubuntu16.04)
    * openmpi3.1.2-ubuntu16.04
- [OpenMPI CPU - Ubuntu 18.04](./base/cpu/openmpi3.1.2-ubuntu18.04)
    * openmpi3.1.2-ubuntu18.04
- [IntelMPI GPU - cuda9 - Ubuntu 16.04](./base/gpu/intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04)
    * base-gpu:intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04
- [OpenMPI GPU - cuda9 - Ubuntu 16.04](./base/gpu/openmpi3.1.2-cuda9.0-cudnn7-ubuntu16.04)
    * openmpi3.1.2-cuda9.0-cudnn7-ubuntu16.04
- [IntelMPI GPU - cuda10.0 - Ubuntu 16.04](./base/gpu/intelmpi2018.3-cuda10.0-cudnn7-ubuntu16.04)
    * intelmpi2018.3-cuda10.0-cudnn7-ubuntu16.04
- [OpenMPI GPU - cuda10.0 - Ubuntu 16.04](./base/gpu/openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04)
    * openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04
- [OpenMPI GPU - cuda10.0 - Ubuntu 18.04](./base/gpu/openmpi3.1.2-cuda10.0-cudnn7-ubuntu18.04)
    * openmpi3.1.2-cuda10.0-cudnn7-ubuntu18.04
- [OpenMPI GPU - cuda10.1 - Ubuntu 18.04](./base/gpu/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04)
    * openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04
- [OpenMPI GPU - cuda10.2 - Ubuntu 18.04](./base/gpu/openmpi3.1.2-cuda10.2-cudnn7-ubuntu18.04)
    * openmpi3.1.2-cuda10.2-cudnn7-ubuntu18.04
- [OpenMPI GPU - cuda10.2 - Ubuntu 18.04](./base/gpu/openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04)
    * openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04
    
<a name="howtorun"></a>
## Using Azure ML base images for training

In some cases, the Azure ML base images will be used by default:

* By default, if no base image is explicitly set by the user for a training run, Azure ML will use the image corresponding to `azureml.core.runconfig.DEFAULT_CPU_IMAGE`.

* If you are using an [Azure ML curated environment](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments#use-a-curated-environment), those are already configured with one of the Azure ML base images. To see which base image is used by a specific curated environment, you can run the following:
     ```python
     from azureml.core import Environment
     
     curated_env_name = 'AzureML-PyTorch-1.6-GPU'
     pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
     print(pytorch_env.docker.base_image)
     ```
     
If you want to instead explicitly use one of the Azure ML base images for your job, you can follow the steps below.

<a name="prerequisites"></a>
### Prerequisites
 * Install Azure ML SDK and [setup environment](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-run-local-notebook)
 * Quickstarts, end-to-end tutorials, and how-tos on the [official documentation site for Azure Machine Learning service](https://docs.microsoft.com/en-us/azure/machine-learning/service/).
 * [Python SDK reference](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/intro?view=azure-ml-py)

<a name="createenv"></a>
### Create an Azure ML Environment

If your training script requires additional dependencies, create a YAML file that defines the conda dependencies. In the below example, the file is named `conda_dependencies.yml`:
```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - tensorflow-gpu==2.2.0
```

Then, create an Azure ML environment from this conda environment specification.
```python
from azureml.core import Environment

env = Environment.from_conda_specification(name='my-env', file_path='./conda_dependencies.yml')
```

If your script does not require any additional dependencies and you would just like to use the base image directly, just instantiate an Environment object with the following:
```python
from azureml.core import Environment

env = Environment(name='my-env')
```

Then, for both of the above cases, set the base image you would like to use. For example, here we will specify the openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04 image:
```python
env.docker.enabled = True
env.docker.base_image = 'mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04'
```

<a name="submitjob"></a>
### Configure and submit the training job
Create a [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) object to specify the configuration details of your training job, including your training script, environment to use, and the compute target to run on. 

```python
from azureml.core import Workspace, Experiment
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
compute_target = ws.compute_targets['my-cluster-name']

src = ScriptRunConfig(source_directory='.',
                      script='train.py',
                      compute_target=compute_target,
                      environment=env)
                      
run = Experiment(workspace=ws, name='my-experiment').submit(src)
run.wait_for_completion(show_output=True)
```

<a name="jobexe"></a>
### What happens during job execution
As the job is executed, it goes through the following stages:

- **Preparing**: A docker image is created according to the environment defined. The image is uploaded to the workspace's Azure Container Registry and cached for later runs. A new Docker image is built if this is the first time a combination of dependencies are used in a workspace. If not, a cached Docker image is used. Logs are also streamed to the run history and can be viewed to monitor progress. If a curated environment is specified instead, the cached image backing that curated environment will be used.

- **Scaling**: The cluster attempts to scale up if the cluster requires more nodes to execute the run than are currently available.

- **Running**: All scripts in the script folder are uploaded to the compute target, any datasets specified are mounted or downloaded, and the `script` is executed. Outputs from stdout and the **./logs** folder are streamed to the run history and can be used to monitor the run.

- **Post-Processing**: The **./outputs** folder of the run is copied over to the run history.
    
<a name="customdocker"></a>
## Using your own custom Docker image or Dockerfile for training
If you instead want to use your own custom Docker image or Dockerfile for your training job instead of the Azure ML base images, you can refer to the documentation [Train using a custom image](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-custom-image).

<a name="resources"></a>
## Resources
For additional documentation and tutorials, see the following:
- [Azure ML sample notebooks for training](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks)
- [Create and manage Azure ML environments](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments)
- [Train TensorFlow models on Azure ML](https://docs.microsoft.com/azure/machine-learning/how-to-train-tensorflow)
- [Train PyTorch models on Azure ML](https://docs.microsoft.com/azure/machine-learning/how-to-train-pytorch)

## Projects using Azure Machine Learning

Visit following repositories to see the projects contributed by Azure ML users:

 - [Fine tune natural language processing models using Azure Machine Learning service](https://github.com/Microsoft/AzureML-BERT)
 - [Fashion MNIST with Azure ML SDK](https://github.com/amynic/azureml-sdk-fashion)


This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
