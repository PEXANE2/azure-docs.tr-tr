---
title: Seçkin ortamlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning bulunan seçkin ortamların toplanması
services: machine-learning
author: luisquintanilla
ms.author: luquinta
ms.reviewer: luquinta
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.date: 09/03/2020
ms.openlocfilehash: 4ae96976f81aab9a0949594551c82d3a3fec4f0a
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662075"
---
# <a name="azure-machine-learning-curated-environments"></a>Azure Machine Learning seçkin ortamlar

Bu makalede, Azure Machine Learning ' deki seçkin ortamlar ve bunlara önceden yüklenmiş paketler ve kanallar listelenmektedir. Seçkin ortamlar Azure Machine Learning tarafından sağlanır ve varsayılan olarak çalışma alanınızda kullanılabilir. Bunlar, önbelleğe alınmış Docker görüntüleri tarafından desteklenir, çalıştırma hazırlık maliyeti azalır ve daha hızlı dağıtım zamanına izin verir. Çeşitli makine öğrenimi çerçevelerini hızlıca kullanmaya başlamak için bu ortamları kullanın.

> [!NOTE]
> Bu liste Eylül 2020 itibariyle güncelleştirilir. En güncel listeyi almak için Python SDK 'sını kullanın. Daha fazla bilgi için [ortamlar makalesine](./how-to-use-environments.md#use-a-curated-environment)bakın.

## <a name="azureml-automl"></a>AzureML-oto ml

**Paket kanalları:**

* Anaconda
* Conda-Forge
* pytorch

**Conda paketleri:**

* python
* numpy
* scikit-learn
* pandas
* Kopyala-xgboost
* fbprophet
* lerin
* setuptools-git
* psutil

**PIP paketleri:**

* azureml-core
* azureml-pipeline-core
* azureml-telemetry
* azureml-varsayılanlar
* azureml-interpret
* azureml-explain-model
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* çıkarım-şema
* Kopyala-cpuınfo

## <a name="azureml-automl-dnn"></a>AzureML-oto ml-DNN

**Paket kanalları:**

* Anaconda
* Conda-Forge
* pytorch

**Conda paketleri:**

* python
* numpy
* scikit-learn
* pandas
* Kopyala-xgboost
* fbprophet
* lerin
* setuptools-git
* pytorch
* cubatoolkit
* psutil

**PIP paketleri:**

* azureml-core
* azureml-pipeline-core
* azureml-telemetry
* azureml-varsayılanlar
* azureml-interpret
* azureml-explain-model
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* çıkarım-şema
* pytorch-dönüştürücüler
* spipb
* en_core_web_sm
* Kopyala-cpuınfo

## <a name="azureml-automl-dnn-gpu"></a>AzureML-oto ml-DNN-GPU

**Paket kanalları:**

* Anaconda
* Conda-Forge
* pytorch

**Conda paketleri:**

* python
* numpy
* scikit-learn
* pandas
* fbprophet
* lerin
* setuptools-git
* pytorch
* cubatoolkit
* psutil

**PIP paketleri:**

* azureml-core
* azureml-pipeline-core
* azureml-telemetry
* azureml-varsayılanlar
* azureml-interpret
* azureml-explain-model
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* çıkarım-şema
* horovod
* pytorch-dönüştürücüler
* spipb
* en_core_web_sm
* Kopyala-cpuınfo

## <a name="azureml-automl-dnn-vision-gpu"></a>AzureML-oto ml-DNN-Vision-GPU

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-veri kümesi-çalışma zamanı
* azureml-contrib-dataset
* azureml-telemetry
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-varsayılanlar
* azureml-interpret
* azureml-explain-model
* azureml-train-automl-runtime
* azureml-train-automl
* azureml-contrib-oto ml-DNN-Vision

## <a name="azureml-automl-gpu"></a>AzureML-oto ml-GPU

**Paket kanalları:**

* Anaconda
* Conda-Forge
* pytorch

**Conda paketleri:**

* python
* numpy
* scikit-learn
* pandas
* fbprophet
* lerin
* setuptools-git
* psutil

**PIP paketleri:**

* azureml-core
* azureml-pipeline-core
* azureml-telemetry
* azureml-varsayılanlar
* azureml-interpret
* azureml-explain-model
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* çıkarım-şema
* Kopyala-cpuınfo

## <a name="azureml-chainer-510-cpu"></a>AzureML-Chainer-5.1.0-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Chainer
* mpi4py

## <a name="azureml-chainer-510-gpu"></a>AzureML-Chainer-5.1.0-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Chainer
* cupy-cuda90
* mpi4py

## <a name="azureml-dask-cpu"></a>AzureML-Dadsk-CPU

**Paket kanalları:**

* Conda-Forge
* pytorch
* varsayılan değerler

**Conda paketleri:**

* python

**PIP paketleri:**

* adlfs
* azureml-core
* azureml-veri kümesi-çalışma zamanı
* davsk [Tamam]
* davsk-ml [Tamam]
* Dağıtılmış
* fastparquet
* fsspec
* joblib
* jupyıterlab
* lz4
* mpi4py
* Mini
* pyarrow

## <a name="azureml-dask-gpu"></a>AzureML-Dadsk-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python
* matplotlib

**PIP paketleri:**

* azureml-varsayılanlar
* adlfs
* azureml-core
* davsk [Tamam]
* davsk-ml [Tamam]
* Dağıtılmış
* fastparquet
* fsspec
* joblib
* jupyıterlab
* lz4
* mpi4py
* Mini
* pyarrow

## <a name="azureml-hyperdrive-forecastdnn"></a>AzureML-Hyperdrive-Forebir DNN

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-pipeline-core
* azureml-telemetry
* azureml-varsayılanlar
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* azureml-contrib-oto ml-DNN-tahmin

## <a name="azureml-minimal"></a>AzureML-en az

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar

## <a name="azureml-pyspark-mmlspark-015"></a>AzureML-PySpark-MmlSpark-0,15

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core

## <a name="azureml-pytorch-10-cpu"></a>AzureML-PyTorch-1,0-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod

## <a name="azureml-pytorch-10-gpu"></a>AzureML-PyTorch-1,0-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod

## <a name="azureml-pytorch-11-cpu"></a>AzureML-PyTorch-1,1-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-11-gpu"></a>AzureML-PyTorch-1,1-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-12-cpu"></a>AzureML-PyTorch-1,2-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-12-gpu"></a>AzureML-PyTorch-1,2-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-13-cpu"></a>AzureML-PyTorch-1,3-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-13-gpu"></a>AzureML-PyTorch-1,3-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-14-cpu"></a>AzureML-PyTorch-1,4-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-14-gpu"></a>AzureML-PyTorch-1,4-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-15-cpu"></a>AzureML-PyTorch-1,5-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-15-gpu"></a>AzureML-PyTorch-1,5-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-16-cpu"></a>AzureML-PyTorch-1,6-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-pytorch-16-gpu"></a>AzureML-PyTorch-1,6-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* Torch
* torchvision
* MKL
* horovod
* tensorboard
* yayımlanacak

## <a name="azureml-scikit-learn-0203"></a>AzureML-Scikit-öğren-0.20.3

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* scikit-learn
* scipy
* joblib

## <a name="azureml-tensorflow-110-cpu"></a>AzureML-TensorFlow-1,10-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-110-gpu"></a>AzureML-TensorFlow-1,10-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tensorflow-112-cpu"></a>AzureML-TensorFlow-1,12-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-112-gpu"></a>AzureML-TensorFlow-1,12-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tensorflow-113-cpu"></a>AzureML-TensorFlow-1,13-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-113-gpu"></a>AzureML-TensorFlow-1,13-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tensorflow-20-cpu"></a>AzureML-TensorFlow-2,0-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-20-gpu"></a>AzureML-TensorFlow-2,0-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tensorflow-21-cpu"></a>AzureML-TensorFlow-2,1-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-21-gpu"></a>AzureML-TensorFlow-2,1-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tensorflow-22-cpu"></a>AzureML-TensorFlow-2,2-CPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow
* horovod

## <a name="azureml-tensorflow-22-gpu"></a>AzureML-TensorFlow-2,2-GPU

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* TensorFlow-GPU
* horovod

## <a name="azureml-tutorial"></a>AzureML-öğretici

**Paket kanalları:**

* Anaconda
* Conda-Forge

**Conda paketleri:**

* python
* pandas
* numpy
* tqdm
* scikit-learn
* matplotlib

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-telemetry
* azureml-tren-restclients-Hyperdrive
* azureml-train-core
* azureml-widgets
* azureml-pipeline-core
* azureml-pipeline-steps
* azureml-opendatasets
* azureml-automl-core
* azureml-automl-runtime
* azureml-train-automl-client
* azureml-train-automl-runtime
* azureml-train-automl
* azureml-eğitme
* azureml-SDK
* azureml-interpret
* azureml-tensorboard
* azureml-mlflow
* mlflow
* sköğren-Pandas

## <a name="azureml-vowpalwabbit-880"></a>AzureML-VowpalWabbit-8.8.0

**Paket kanalları:**

* Conda-Forge

**Conda paketleri:**

* python

**PIP paketleri:**

* azureml-core
* azureml-varsayılanlar
* azureml-veri kümesi-çalışma zamanı [sigortası, Pandas]
