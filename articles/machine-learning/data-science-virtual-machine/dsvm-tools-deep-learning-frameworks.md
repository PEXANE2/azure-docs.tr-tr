---
title: Derin Öğrenme & AI çerçeveleri
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makine'de kullanılabilir derin öğrenme çerçeveleri ve araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: d8a5cf428f41b130e6faf68ac87a075c15211099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270075"
---
# <a name="deep-learning-and-ai-frameworks-for-the-azure-data-science-vm"></a>Azure Veri Bilimi VM için derin öğrenme ve AI çerçeveleri
DSVM üzerinde derin öğrenme çerçeveleri aşağıda listelenmiştir.

## <a name="caffe"></a>[Caffe](https://github.com/BVLC/caffe)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl yapılandırılır / DSVM yüklü?  | Caffe yüklü `/opt/caffe`.   Örnekler içinde. `/opt/caffe/examples`|
| Nasıl çalıştırılır?      | VM'nizde oturum kurmak için X2Go'yu kullanın ve ardından yeni bir terminal başlatın ve aşağıdakileri girin:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Örnek not defterleriyle yeni bir tarayıcı penceresi açılır. İkili /opt/caffe/build/install/bin'e yüklenir.<br/><br/>Caffe'nin yüklü sürümü Python 2.7 gerektirir ve varsayılan olarak etkinleştirilen Python 3.5 ile çalışmaz. Python 2.7'ye geçmek `source activate root` için Anaconda ortamına geçmek için çalıştırın.|    

## <a name="caffe2"></a>[Caffe2](https://github.com/caffe2/caffe2)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl yapılandırılır / DSVM yüklü?  | Caffe2 [Python 2.7 (root) conda ortamında yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python'u başlatın ve Caffe2'yi aktarın. <br/> * JupyterHub: [JupyterHub bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve sonra örnek dizüstü bilgisayarlar bulmak için Caffe2 dizinine gidin. Bazı not defterleri, Python kodunda ayarlanacak Caffe2 kökünü gerektirir; /opt/caffe2 girin. |

## <a name="chainer"></a>[Chainer](https://chainer.org/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 5.2 |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl yapılandırılır / DSVM yüklü?  | Chainer Python 3.5'e yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python 3.5 ortamını `python`etkinleştirin, çalıştırın ve sonra `import chainer`. <br/> * JupyterHub: [JupyterHub bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve sonra örnek dizüstü bilgisayarlar bulmak için Chainer dizinine gidin.| 

## <a name="cuda-cudnn-nvidia-driver"></a>[CUDA, cuDNN, NVIDIA Sürücüsü](https://developer.nvidia.com/cuda-toolkit)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 10.0.130|
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  |_nvidia-smi_ sistem yolunda mevcuttur.  |
| Nasıl çalıştırılır?      | Bir komut istemi (Windows' da) veya terminal (Linux'ta) açın ve ardından _nvidia-smi_çalıştırın. |


## <a name="horovod"></a>[Horovod](https://github.com/uber/horovod)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 0.16.1|
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)   |
| Nasıl yapılandırılır / DSVM yüklü?  | Horovod Python 3.5 yüklü |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python çalıştırın. |

## <a name="keras"></a>[Keras](https://keras.io/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 2.2.4 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  | Keras, Python 3.6'da Windows'da ve Python 3.5'te Linux'ta yüklenir |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python çalıştırın. |

## <a name="microsoft-cognitive-toolkit-cntk"></a>[Microsoft Bilişsel Araç Seti (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 2.5.1 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  | CNTK, Python 3.6'da [Windows 2016'da](dsvm-tools-languages.md#python-windows-server-2016-edition) ve Python 3.5'te [Linux'ta](./dsvm-tools-languages.md#python-linux-edition)yüklenir) |
| Nasıl çalıştırılır?      | Terminal: Doğru ortamı etkinleştirin ve Python çalıştırın. <br/>Jupyter: [Jupyter](provision-vm.md) veya [JupyterHub'a](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)bağlanın ve ardından cntk dizinini örnekler için açın. |

## <a name="mxnet"></a>[MXNet](https://mxnet.apache.org/)
|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.3.0 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  | MXNet Windows ve `C:\dsvm\tools\mxnet` `/dsvm/tools/mxnet` Ubuntu'da yüklenir. Python bağlamaları [Windows 2016'da](dsvm-tools-languages.md#python-windows-server-2016-edition) Python 3.6'ya ve [Linux'ta](./dsvm-tools-languages.md#python-linux-edition)Python 3.5'te yüklenir) R bağlamaları ubuntu DSVM'ye de dahildir. |
| Nasıl çalıştırılır?      | Terminal: Doğru conda ortamını `import mxnet`etkinleştirin, ardından çalıştırın. <br/>Jupyter: [Jupyter](provision-vm.md#access-the-dsvm) veya [JupyterHub'a](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)bağlanın ve ardından örnekler için dizini `mxnet` açın. |

## <a name="mxnet-model-server"></a>[MXNet Model Server](https://github.com/awslabs/mxnet-model-server#quick-start)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.0.1 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  | MXNet Model Server, Python 3.6'da [Windows 2016'da](dsvm-tools-languages.md#python-windows-server-2016-edition) ve Python 3.5'te [Linux'ta](./dsvm-tools-languages.md#python-linux-edition)yüklenir) |
| Nasıl çalıştırılır?      | Terminal: `sudo systemctl stop jupyterhub` Her ikisi de aynı bağlantı noktasında dinlediği için önce JupyterHub hizmetini durdurmak için çalıştırın. Sonra doğru conda ortamı etkinleştirin ve çalıştırın`mxnet-model-server --start --models squeezenet=https://s3.amazonaws.com/model-server/model_archive_1.0/squeezenet_v1.1.mar` |

## <a name="nvidia-system-management-interface-nvidia-smi"></a>[NVidia Sistem Yönetim Arayüzü (nvidia-smi)](https://developer.nvidia.com/nvidia-system-management-interface)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli |  |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Ne için? | GPU etkinliğini sorgulamak için NVIDIA aracı |
| Nasıl yapılandırılır / DSVM yüklü?  | `nvidia-smi`sistem yolu üzerindedir. |
| Nasıl çalıştırılır?      | **GPU'lu**sanal bir makinede, bir komut istemi (Windows'ta) veya terminal `nvidia-smi`(Linux'ta) açın ve ardından çalıştırın. |

## <a name="pytorch"></a>[PyTorch](https://pytorch.org/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.2.0 (Ubuntu 16.04, Windows 2016), 1.4.0 (Ubuntu 18.04, Windows 2019) |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl yapılandırılır / DSVM yüklü?  | [Python 3.5](dsvm-tools-languages.md#python-linux-edition)yüklü . Örnek Jupyter dizüstü bilgisayarlar dahildir ve örnekler /dsvm/samples/pytorch'dadır. |
| Nasıl çalıştırılır?      | Terminal: Doğru ortamı etkinleştirin ve python çalıştırın.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): Bağlanın ve örnekler için PyTorch dizinini açın.  |

## <a name="tensorflow"></a>[TensorFlow](https://www.tensorflow.org/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.13 |
| Desteklenen DSVM sürümleri      | Windows, Linux |
| Nasıl yapılandırılır / DSVM yüklü?  | [Windows 2016'da](dsvm-tools-languages.md#python-windows-server-2016-edition) [Linux](dsvm-tools-languages.md#python-linux-edition) ve Python 3.6'da Python 3.5 yüklü |
| Nasıl çalıştırılır?      | Terminal: Doğru ortamı etkinleştirin ve python çalıştırın. <br/> * Jupyter: [Jupyter](provision-vm.md) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)bağlanın ve sonra örnekler için TensorFlow dizinini açın.   |

## <a name="tensorflow-serving"></a>[TensorFlow Serving](https://www.tensorflow.org/serving/)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.12 |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl yapılandırılır / DSVM yüklü?  | tensorflow_model_server terminalde mevcuttur. |
| Nasıl çalıştırılır?      |  Örnekler [online](https://www.tensorflow.org/serving/)olarak mevcuttur.   |


## <a name="theano"></a>[Theano](https://github.com/Theano/Theano)

|    |           |
| ------------- | ------------- |
| Sürüm(ler) destekli | 1.0.3 |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl yapılandırılır / DSVM yüklü?  |Theano Python 2.7 _(kök)_ ve Python 3.5 _(py35_) ortamında yüklenir. |
| Nasıl çalıştırılır?      |  Terminal: İstediğindiğiniz Python sürümünü etkinleştirin (root veya py35), Python'u çalıştırın ve theano'yu içe aktarın.<br/>* Jupyter: Python 2.7 veya 3.5 çekirdeğini seçin ve sonra Theano'yu aktarın.  <br/>Yeni bir matematik çekirdeği kitaplığı (MKL) hatasını aşmak için öncelikle MKL iş parçacığı katmanını aşağıdaki gibi ayarlamanız gerekir:<br/><br/>`export MKL_THREADING_LAYER=GNU`  |