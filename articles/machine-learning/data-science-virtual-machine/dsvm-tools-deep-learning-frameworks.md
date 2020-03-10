---
title: Ayrıntılı öğrenme & AI çerçeveleri
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi 'de kullanılabilir derin öğrenme çerçeveleri ve araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: d8a5cf428f41b130e6faf68ac87a075c15211099
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78390671"
---
# <a name="deep-learning-and-ai-frameworks-for-the-azure-data-science-vm"></a>Azure Veri Bilimi VM'si için derin öğrenme ve AI çerçeveleri
DSVM üzerindeki derin öğrenme çerçeveleri aşağıda listelenmiştir.

## <a name="caffe"></a>[Caffe](https://github.com/BVLC/caffe)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Caffe, `/opt/caffe`yüklendi.   Örnekler `/opt/caffe/examples`.|
| Nasıl çalıştırılır?      | X2Go kullanarak sanal makinenizde oturum açın ve ardından yeni bir Terminal başlatın ve aşağıdakileri girin:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Örnek Not Defterleri ile yeni bir tarayıcı penceresi açılır. İkili dosyaları içinde /opt/caffe/build/install/bin yüklenir.<br/><br/>Caffe 'nin yüklü sürümü Python 2,7 gerektirir ve varsayılan olarak etkinleştirilen Python 3,5 ile çalışmaz. Python 2,7 ' ye geçmek için `source activate root` çalıştırarak Anaconda ortamına geçin.|    

## <a name="caffe2"></a>[Caffe2](https://github.com/caffe2/caffe2)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Caffe2, [Python 2,7 (root) Conda ortamına yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python 'u başlatın ve Caffe2 içeri aktarın. <br/> * JupyterHub: [jupyterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve ardından örnek not defterlerini bulmak için Caffe2 dizinine gidin. Bazı dizüstü bilgisayarlar Python kodunda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin. |

## <a name="chainer"></a>[Chainer](https://chainer.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 5.2 |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Chainer, Python 3,5 ' ye yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python 3,5 ortamını etkinleştirin, `python`çalıştırın ve `import chainer`. <br/> * JupyterHub: [jupyterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve ardından örnek not defterlerini bulmak Için Chainer dizinine gidin.| 

## <a name="cuda-cudnn-nvidia-driver"></a>[CUDA, cuDNN, NVıDıA sürücüsü](https://developer.nvidia.com/cuda-toolkit)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 10.0.130|
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  |_NVIDIA-SMI_ sistem yolunda kullanılabilir.  |
| Nasıl çalıştırılır?      | Bir komut istemi (Windows üzerinde) veya bir Terminal (Linux 'ta) açın ve ardından _NVIDIA-SMI_' yi çalıştırın. |


## <a name="horovod"></a>[Horovod](https://github.com/uber/horovod)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 0.16.1|
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Horovod, Python 3,5 ' de yüklü |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python 'u çalıştırın. |

## <a name="keras"></a>[Keras](https://keras.io/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 2.2.4 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Keras, Python 3,6 ' ye Windows üzerinde ve Linux 'ta Python 3,5 ' de yüklendi |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python 'u çalıştırın. |

## <a name="microsoft-cognitive-toolkit-cntk"></a>[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 2.5.1 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | CNTK, [Windows 2016](dsvm-tools-languages.md#python-windows-server-2016-edition) ' de Python 3,6 ' de ve [Linux](./dsvm-tools-languages.md#python-linux-edition)üzerinde Python 3,5 ' de yüklüdür) |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve Python 'U çalıştırın. <br/>Jupyter: [Jupyter](provision-vm.md) veya [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)'a bağlanın ve ardından örnekler için cntk dizinini açın. |

## <a name="mxnet"></a>[MXNet](https://mxnet.apache.org/)
|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.3.0 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | MXNet, Windows üzerinde `C:\dsvm\tools\mxnet` ve Ubuntu üzerinde `/dsvm/tools/mxnet` yüklenir. Python bağlamaları [Windows 2016](dsvm-tools-languages.md#python-windows-server-2016-edition) ' de Python 3,6 ' ye yüklenir ve [Linux](./dsvm-tools-languages.md#python-linux-edition)üzerinde Python 3,5 ' de bulunur), Ubuntu dsvm 'ye de R bağlamaları dahildir. |
| Nasıl çalıştırılır?      | Terminal: doğru Conda ortamını etkinleştirin ve `import mxnet`çalıştırın. <br/>Jupyter: [Jupyter](provision-vm.md#access-the-dsvm) veya [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)'a bağlanın ve ardından örnekler için `mxnet` dizinini açın. |

## <a name="mxnet-model-server"></a>[MXNet model sunucusu](https://github.com/awslabs/mxnet-model-server#quick-start)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.0.1 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | MXNet model sunucusu, [Windows 2016](dsvm-tools-languages.md#python-windows-server-2016-edition) ' de Python 3,6 ' de ve [Linux](./dsvm-tools-languages.md#python-linux-edition)üzerinde Python 3,5 ' de yüklüdür) |
| Nasıl çalıştırılır?      | Terminal: her ikisi de aynı bağlantı noktasını dinlemek için JupyterHub hizmetini durdurmak üzere `sudo systemctl stop jupyterhub` çalıştırın. Ardından, doğru Conda ortamını etkinleştirin ve `mxnet-model-server --start --models squeezenet=https://s3.amazonaws.com/model-server/model_archive_1.0/squeezenet_v1.1.mar` çalıştırın |

## <a name="nvidia-system-management-interface-nvidia-smi"></a>[NVIDIA sistem yönetimi arabirimi (NVIDIA-SMI)](https://developer.nvidia.com/nvidia-system-management-interface)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) |  |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| Ne için? | NVIDIA GPU etkinliği sorgulama aracı |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | `nvidia-smi` sistem yolunuzda. |
| Nasıl çalıştırılır?      | **GPU 'nun bulunduğu**bir sanal makinede, bir komut istemi açın (Windows üzerinde) veya bir Terminal (Linux 'ta) ve ardından `nvidia-smi`çalıştırın. |

## <a name="pytorch"></a>[PyTorch](https://pytorch.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.2.0 (Ubuntu 16,04, Windows 2016), 1.4.0 (Ubuntu 18,04, Windows 2019) |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | [Python 3,5](dsvm-tools-languages.md#python-linux-edition)' de yüklü. Örnek Jupkiter Not defterleri dahil edilmiştir ve örnekler/dsvm/Samples/pytorch. |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın.<br/>* [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): Connect, ve sonra örnekler Için PyTorch dizinini açın.  |

## <a name="tensorflow"></a>[TensorFlow](https://www.tensorflow.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1,13 |
| Desteklenen DSVM sürümleri      | Windows, Linux |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | [Windows 2016](dsvm-tools-languages.md#python-windows-server-2016-edition) üzerinde [Linux](dsvm-tools-languages.md#python-linux-edition) ve Python 3,6 ' de Python 3,5 ' ye yüklendi |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın. <br/> * Jupyter: [Jupyter](provision-vm.md) veya [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)'a bağlanın ve örnekler için TensorFlow dizinini açın.   |

## <a name="tensorflow-serving"></a>[TensorFlow hizmeti](https://www.tensorflow.org/serving/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1,12 |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | tensorflow_model_server terminalde mevcuttur. |
| Nasıl çalıştırılır?      |  Örnekler [çevrimiçi](https://www.tensorflow.org/serving/)olarak kullanılabilir.   |


## <a name="theano"></a>[Theano](https://github.com/Theano/Theano)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.0.3 |
| Desteklenen DSVM sürümleri      | Linux |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  |Bu, Python 2,7 ' de (_root_) ve Python 3,5 (_py35_) ortamında yüklenir. |
| Nasıl çalıştırılır?      |  Terminal: istediğiniz Python sürümünü etkinleştirin (root veya py35), Python 'u çalıştırın ve ardından Ano içeri aktarın.<br/>* Jupyter: Python 2,7 veya 3,5 çekirdeğini seçin ve sonra da Ano içeri aktarın.  <br/>Son matematik çekirdek kitaplığı (MKL) hatasını geçici olarak çözmek için öncelikle MKL iş parçacığı katmanını aşağıdaki şekilde ayarlamanız gerekir:<br/><br/>`export MKL_THREADING_LAYER=GNU`  |