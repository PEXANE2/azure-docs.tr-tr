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
ms.topic: conceptual
ms.date: 07/08/2020
ms.openlocfilehash: 9d0964fb28451abe0a77276d6f9d605fcaac2514
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156446"
---
# <a name="azure-machine-learning-curated-environments"></a>Azure Machine Learning seçkin ortamlar

Bu makalede, Azure Machine Learning ' deki seçkin ortamlar ve bunlara önceden yüklenmiş paketler ve kanallar listelenmektedir.

> [!NOTE]
> Bu liste Haziran 2020 itibariyle güncelleştirilir. En güncel listeyi almak için Python SDK 'sını kullanın. Daha fazla bilgi için [ortamlar makalesine](./how-to-use-environments.md#use-a-curated-environment)bakın.

## <a name="azure-automl"></a>Azure oto ml

- [AzureML oto ml](#azureml-automl)
- [AzureML bir oto GPU](#azureml-automl-gpu)
- [AzureML oto ml DNN](#azureml-automl-dnn)
- [AzureML oto ml DNN GPU 'SU](#azureml-automl-dnn-gpu)
- [Azure oto ml DNN Vision GPU](#azureml-automl-dnn-vision-gpu)

### <a name="azureml-automl"></a>AzureML oto ml

paket kanalları:
- Anaconda
- Conda-Forge
- pytorch

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-açıkla-model = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - çıkarım-şema
  - pyarrow = = 0.17.0
  - Kopyala-cpuınfo = = 5.0.0
- sayısal tuş>= 1.16.0, <= 1.16.2
- Pandas>= 0.21.0, <= 0.23.4
- Kopyala-xgboost<= 0,90
- fbprophet = = 0,5
- setuptools-git
- psutil>5.0.0, <6.0.0

### <a name="azureml-automl-gpu"></a>AzureML bir oto GPU

paket kanalları:
- Anaconda
- Conda-Forge
- pytorch

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-açıkla-model = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - çıkarım-şema
  - pyarrow = = 0.17.0
  - Kopyala-cpuınfo = = 5.0.0
- sayısal tuş>= 1.16.0, <= 1.16.2
- Pandas>= 0.21.0, <= 0.23.4
- fbprophet = = 0,5
- setuptools-git
- psutil>5.0.0, <6.0.0

### <a name="azureml-automl-dnn"></a>AzureML oto ml DNN

paket kanalları:
- Anaconda
- Conda-Forge
- pytorch

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-açıkla-model = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - çıkarım-şema
  - pytorch-dönüştürücüler = = 1.0.0
  - spipb = = 2.1.8
  - https://aka.ms/automl-resources/packages/en_core_web_sm-2.1.0.tar.gz
  - pyarrow = = 0.17.0
  - Kopyala-cpuınfo = = 5.0.0
- Pandas>= 0.21.0, <= 0.23.4
- sayısal tuş>= 1.16.0, <= 1.16.2
- Kopyala-xgboost<= 0,90
- fbprophet = = 0,5
- setuptools-git
- pytorch = 1.4.0
- cubatoolkit = 10.0.130
- psutil>5.0.0, <6.0.0

### <a name="azureml-automl-dnn-gpu"></a>AzureML oto ml DNN GPU 'SU

paket kanalları:
- Anaconda
- Conda-Forge
- pytorch

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-açıkla-model = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - çıkarım-şema
  - horovod = = 0.19.4
  - pytorch-dönüştürücüler = = 1.0.0
  - spipb = = 2.1.8
  - https://aka.ms/automl-resources/packages/en_core_web_sm-2.1.0.tar.gz
  - pyarrow = = 0.17.0
  - Kopyala-cpuınfo = = 5.0.0
- Pandas>= 0.21.0, <= 0.23.4
- sayısal tuş>= 1.16.0, <= 1.16.2
- fbprophet = = 0,5
- setuptools-git
- pytorch = 1.4.0
- cubatoolkit = 10.0.130
- psutil>5.0.0, <6.0.0

### <a name="azureml-automl-dnn-vision-gpu"></a>AzureML oto ml DNN Vision GPU

bağlantılıdır
- Python = 3.7
- Pip
  - azureml-oto ml-Core = = 1.8.0
  - azureml-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-açıkla-model = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - azureml-eğitme-oto ml = = 1.8.0
  - azureml-contrib-DataSet = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - azureml-işlem hattı-adımlar = = 1.8.0
  - azureml-işlem hattı = = 1.8.0
  - azureml-eğitme = = 1.8.0
  - azureml-SDK = = 1.8.0
  - azureml-contrib-oto ml-DNN-Vision = = 1.8.0

## <a name="azure-ml-designer"></a>Azure ML Tasarımcısı

- [AzureML Tasarımcısı](#azureml-designer)
- [AzureML Tasarımcısı CV](#azureml-designer-cv)
- [AzureML Tasarımcısı CV dönüşümü](#azureml-designer-cv-transform)
- [AzureML tasarımcı GÇ](#azureml-designer-io)
- [AzureML Tasarımcısı NLP](#azureml-designer-nlp)
- [AzureML Tasarımcısı PyTorch](#azureml-designer-pytorch)
- [AzureML Tasarımcısı PyTorch eğitme](#azureml-designer-pytorch-train)
- [AzureML Tasarımcısı R](#azureml-designer-r)
- [AzureML Tasarımcısı öneren](#azureml-designer-recommender)
- [AzureML tasarımcı puanı](#azureml-designer-score)
- [AzureML tasarımcı dönüşümü](#azureml-designer-transform)

### <a name="azureml-designer"></a>AzureML Tasarımcısı

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.8
- scikit-sürpriz = 1.0.6
- Pip
  - azureml-tasarımcı-klasik-modüller = = 0.0.124
  - https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-2.1.0/en_core_web_sm-2.1.0.tar.gz#egg=en_core_web_sm
  - spipb = = 2.1.7

### <a name="azureml-designer-cv"></a>AzureML Tasarımcısı CV

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-CV-modüller = = 0.0.6

### <a name="azureml-designer-cv-transform"></a>AzureML Tasarımcısı CV dönüşümü

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-CV-modüller [pytorch] = = 0.0.6

### <a name="azureml-designer-io"></a>AzureML tasarımcı GÇ

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-dataprep>= 1,6
  - azureml-tasarımcı-dataıo-modüller = = 0.0.30

### <a name="azureml-designer-nlp"></a>AzureML Tasarımcısı NLP

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-klasik-modüller = = 0.0.121
  - https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-2.1.0/en_core_web_sm-2.1.0.tar.gz#egg=en_core_web_sm
  - spipb = = 2.1.7

### <a name="azureml-designer-pytorch"></a>AzureML Tasarımcısı PyTorch

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-pytorch-modüller = = 0.0.8

### <a name="azureml-designer-pytorch-train"></a>AzureML Tasarımcısı PyTorch eğitme

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-pytorch-modüller = = 0.0.8

### <a name="azureml-designer-r"></a>AzureML Tasarımcısı R

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.8
- r-şapka = 6.0
- r-catools = 1.17.1
- r-Cluster = 2.1.0
- r-dplyr = 0.8.5
- r-e1071 = 1.7
- r-forkediler = 0.5.0
- r-tahmin = 8.12
- r-glmnet = 2.0
- r-igraph = 1.2.4
- r-matris = 1.2
- r-mclust = 5.4.6
- r-mgcv = 1.8
- r-nlme = 3.1
- r-nnet = 7.3
- r-plyr = 1.8.6
- r-randomforest = 4.6
- r-reticulate = 1.12
- r-rocr = 1.0
- r-rodbc = 1.3
- r-rpart = 4.1
- r-stringr = 1.4.0
- r-tidyverse = 1.2.1
- r-timedate = 3043.102
- r-Tseries = 0.10
- r = 3.5.1
- Pip
  - azureml-tasarımcı-klasik-modüller = = 0.0.124

### <a name="azureml-designer-recommender"></a>AzureML Tasarımcısı öneren

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-öneren-modüller = = 0.0.5

### <a name="azureml-designer-score"></a>AzureML tasarımcı puanı

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Conda
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-skor-modüller = = 0.0.5

### <a name="azureml-designer-transform"></a>AzureML tasarımcı dönüşümü

paket kanalları:
- varsayılan değerler

bağlantılıdır
- Python = 3.6.8
- Pip
  - azureml-tasarımcı-DataTransform-modüller = = 0.0.49

## <a name="azureml-hyperdrive-forecastdnn"></a>AzureML Hyperdrive Foreon DNN

bağlantılıdır
- Python = 3.7
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1
  - azureml-contrib-oto ml-DNN-tahmin = = 1.8.0

## <a name="azureml-minimal"></a>AzureML en az

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0

## <a name="azureml-sidecar"></a>AzureML arabası

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2

## <a name="azureml-tutorial"></a>AzureML öğreticisi

paket kanalları:
- Anaconda
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - azureml-pencere öğeleri = = 1.8.0
  - azureml-işlem hattı-çekirdek = = 1.8.0
  - azureml-işlem hattı-adımlar = = 1.8.0
  - azureml-OPENDATASET = = 1.8.0
  - azureml-oto ml-Core = = 1.8.0
  - azureml-oto ml-Runtime = = 1.8.0
  - azureml-tren-oto ml-istemci = = 1.8.0
  - azureml-tren-oto ml-Runtime = = 1.8.0. post1  
  - azureml-eğitme-oto ml = = 1.8.0
  - azureml-eğitme = = 1.8.0
  - azureml-SDK = = 1.8.0
  - azureml-yorumlama = = 1.8.0
  - azureml-tensorboard = = 1.8.0
  - azureml-mlflow = = 1.8.0
  - mlflow
  - sköğren-Pandas
- pandas
- numpy
- tqdm
- scikit-learn
- matplotlib

## <a name="azureml-vowpalwabbit-880"></a>AzureML VowpalWabbit 8.8.0

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-dataprep [sigortası, Pandas]

## <a name="dask"></a>Davsk

- [AzureML Davsk CPU 'SU](#azureml-dask-cpu)
- [AzureML Davsk GPU 'SU](#azureml-dask-gpu)

### <a name="azureml-dask-cpu"></a>AzureML Davsk CPU 'SU

paket kanalları:
- Conda-Forge
- pytorch
- varsayılan değerler

bağlantılıdır
- Python = 3.6.9
- Pip
  - adlfs
  - azureml-çekirdek = = 1.8.0
  - azureml-dataprep
  - davsk [Tamam]
  - davsk-ml [Tamam]
  - Dağıtılmış
  - fastparquet
  - fsspec
  - joblib
  - jupyıterlab
  - lz4
  - mpi4py
  - Mini
  - pyarrow

### <a name="azureml-dask-gpu"></a>AzureML Davsk GPU 'SU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.9
- Pip
  - azureml-varsayılanlar = = 1.8.0
  - adlfs
  - azureml-çekirdek = = 1.8.0
  - azureml-dataprep
  - davsk [Tamam]
  - davsk-ml [Tamam]
  - Dağıtılmış
  - fastparquet
  - fsspec
  - joblib
  - jupyıterlab
  - lz4
  - mpi4py
  - Mini
  - pyarrow
- matplotlib

## <a name="chainer"></a>Chainer

- [AzureML Chainer 5.1.0 CPU 'SU](#azureml-chainer-510-cpu)
- [AzureML Chainer 5.1.0 GPU](#azureml-chainer-510-gpu)

### <a name="azureml-chainer-510-cpu"></a>AzureML Chainer 5.1.0 CPU 'SU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Chainer = = 5.1.0
  - mpi4py = = 3.0.0

### <a name="azureml-chainer-510-gpu"></a>AzureML Chainer 5.1.0 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Chainer = = 5.1.0
  - cupy-cuda90 = = 5.1.0
  - mpi4py = = 3.0.0

## <a name="pyspark"></a>PySpark

### <a name="azureml-pyspark-mmlspark-015"></a>AzureML PySpark MmlSpark 0,15

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0

## <a name="pytorch"></a>PyTorch

- [AzureML PyTorch 1,0 CPU](#azureml-pytorch-10-cpu)
- [AzureML PyTorch 1,0 GPU](#azureml-pytorch-10-gpu)
- [AzureML PyTorch 1,1 CPU](#azureml-pytorch-11-cpu)
- [AzureML PyTorch 1,1 GPU](#azureml-pytorch-11-gpu)
- [AzureML PyTorch 1,2 CPU](#azureml-pytorch-12-cpu)
- [AzureML PyTorch 1,2 GPU](#azureml-pytorch-12-gpu)
- [AzureML PyTorch 1,3 CPU](#azureml-pytorch-13-cpu)
- [AzureML PyTorch 1,3 GPU](#azureml-pytorch-13-gpu)
- [AzureML PyTorch 1,4 CPU](#azureml-pytorch-14-cpu)
- [AzureML PyTorch 1,4 GPU](#azureml-pytorch-14-gpu)
- [AzureML PyTorch 1,5 CPU](#azureml-pytorch-15-cpu)
- [AzureML PyTorch 1,5 GPU](#azureml-pytorch-15-gpu)

### <a name="azureml-pytorch-10-cpu"></a>AzureML PyTorch 1,0 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.0
  - torchvision = = 0.2.1
  - MKL = = 2018.0.3
  - horovod = = 0.16.1

### <a name="azureml-pytorch-10-gpu"></a>AzureML PyTorch 1,0 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.0
  - torchvision = = 0.2.1
  - MKL = = 2018.0.3
  - horovod = = 0.16.1

### <a name="azureml-pytorch-11-cpu"></a>AzureML-PyTorch 1,1 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.1
  - torchvision = = 0.2.1
  - MKL = = 2018.0.3
  - horovod = = 0.16.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-11-gpu"></a>AzureML PyTorch 1,1 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.1
  - torchvision = = 0.2.1
  - MKL = = 2018.0.3
  - horovod = = 0.16.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-12-cpu"></a>AzureML PyTorch 1,2 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.2
  - torchvision = = 0.4.0
  - MKL = = 2018.0.3
  - horovod = = 0.16.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-12-gpu"></a>AzureML PyTorch 1,2 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.2
  - torchvision = = 0.4.0
  - MKL = = 2018.0.3
  - horovod = = 0.16.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-13-cpu"></a>AzureML PyTorch 1,3 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.3
  - torchvision = = 0.4.1
  - MKL = = 2018.0.3
  - horovod = = 0.18.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-13-gpu"></a>AzureML PyTorch 1,3 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.3
  - torchvision = = 0.4.1
  - MKL = = 2018.0.3
  - horovod = = 0.18.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-14-cpu"></a>AzureML PyTorch 1,4 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.4.0
  - torchvision = = 0.5.0
  - MKL = = 2018.0.3
  - horovod = = 0.18.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-14-gpu"></a>AzureML PyTorch 1,4 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.4.0
  - torchvision = = 0.5.0
  - MKL = = 2018.0.3
  - horovod = = 0.18.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-15-cpu"></a>AzureML PyTorch 1,5 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.5.0
  - torchvision = = 0.5.0
  - MKL = = 2018.0.3
  - horovod = = 0.19.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

### <a name="azureml-pytorch-15-gpu"></a>AzureML PyTorch 1,5 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - Torch = = 1.5.0
  - torchvision = = 0.5.0
  - MKL = = 2018.0.3
  - horovod = = 0.19.1
  - tensorboard = = 1.14.0
  - gelecek = = 0.17.1

## <a name="scikit-learn"></a>Scikit öğreni

### <a name="azureml-scikit-learn-0203"></a>AzureML Scikit-0.20.3 öğrenin

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - scikit-öğren = = 0.20.3
  - SciPy = = 1.2.1
  - joblib = = 0.13.2

## <a name="tensorflow"></a>TensorFlow

- [Azure ML TensorFlow 1,10 CPU](#azureml-tensorflow-110-cpu)
- [Azure ML TensorFlow 1,10 GPU](#azureml-tensorflow-110-gpu)
- [Azure ML TensorFlow 1,12 CPU](#azureml-tensorflow-112-cpu)
- [Azure ML TensorFlow 1,12 GPU](#azureml-tensorflow-112-gpu)
- [Azure ML TensorFlow 1,13 CPU](#azureml-tensorflow-113-cpu)
- [Azure ML TensorFlow 1,13 GPU](#azureml-tensorflow-113-gpu)
- [Azure ML TensorFlow 2,0 CPU](#azureml-tensorflow-20-cpu)
- [Azure ML TensorFlow 2,0 GPU](#azureml-tensorflow-20-gpu)
- [Azure ML TensorFlow 2,1 CPU](#azureml-tensorflow-21-cpu)
- [Azure ML TensorFlow 2,1 GPU](#azureml-tensorflow-21-gpu)

### <a name="azureml-tensorflow-110-cpu"></a>AzureML TensorFlow 1,10 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow = = 1,10
  - horovod = = 0.15.2

### <a name="azureml-tensorflow-110-gpu"></a>AzureML TensorFlow 1,10 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow-GPU = = 1.10.0
  - horovod = = 0.15.2

### <a name="azureml-tensorflow-112-cpu"></a>AzureML TensorFlow 1,12 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow = = 1.12
  - horovod = = 0.15.2

### <a name="azureml-tensorflow-112-gpu"></a>AzureML TensorFlow 1,12 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow-GPU = = 1.12.0
  - horovod = = 0.15.2

### <a name="azureml-tensorflow-113-cpu"></a>AzureML-TensorFlow 1,13 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow = = 1.13.1
  - horovod = = 0.16.1

### <a name="azureml-tensorflow-113-gpu"></a>AzureML TensorFlow 1,13 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow-GPU = = 1.13.1
  - horovod = = 0.16.1

### <a name="azureml-tensorflow-20-cpu"></a>AzureML TensorFlow 2,0 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow = = 2.0
  - horovod = = 0.18.1

### <a name="azureml-tensorflow-20-gpu"></a>AzureML TensorFlow 2,0 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow-GPU = = 2.0.0
  - horovod = = 0.18.1

### <a name="azureml-tensorflow-21-cpu"></a>AzureML TensorFlow 2,1 CPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow = = 2.1.0
  - horovod = = 0.19.1

### <a name="azureml-tensorflow-21-gpu"></a>AzureML TensorFlow 2,1 GPU

paket kanalları:
- Conda-Forge

bağlantılıdır
- Python = 3.6.2
- Pip
  - azureml-çekirdek = = 1.8.0
  - azureml-varsayılanlar = = 1.8.0
  - azureml-telemetri = = 1.8.0
  - azureml-tren-restclients-Hyperdrive = = 1.8.0
  - azureml-tren-Core = = 1.8.0
  - TensorFlow-GPU = = 2.1.0
  - horovod = = 0.19.1

