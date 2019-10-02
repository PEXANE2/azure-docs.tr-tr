---
title: Ayrıntılı öğrenme & AI çerçeveleri
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi 'de kullanılabilir derin öğrenme çerçeveleri ve araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/1/2019
ms.openlocfilehash: fd38bf1f7741c4d610ef43a12d90533d4ac7b703
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802423"
---
# <a name="deep-learning-and-ai-frameworks-for-the-azure-data-science-vm"></a>Azure Veri Bilimi VM'si için derin öğrenme ve AI çerçeveleri
DSVM üzerindeki derin öğrenme çerçeveleri aşağıda listelenmiştir.

## <a name="caffehttpsgithubcombvlccaffe"></a>[Caffe](https://github.com/BVLC/caffe)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | Caffe `/opt/caffe` ' a yüklenir.   Örnekler `/opt/caffe/examples` ' dır.|
| Nasıl çalıştırılır?      | X2Go kullanarak sanal makinenizde oturum açın ve ardından yeni bir Terminal başlatın ve aşağıdakileri girin:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Örnek Not defterleri ile yeni bir tarayıcı penceresi açılır. İkililer/seçenek/Caffe/Build/Install/bina 'da yüklenir.<br/><br/>Caffe 'nin yüklü sürümü Python 2,7 gerektirir ve varsayılan olarak etkinleştirilen Python 3,5 ile çalışmaz. Python 2,7 ' ye geçmek için, Anaconda ortamına geçmek için `source activate root` ' ı çalıştırın.|    

## <a name="caffe2httpsgithubcomcaffe2caffe2"></a>[Caffe2](https://github.com/caffe2/caffe2)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | Caffe2, [Python 2,7 (root) Conda ortamına yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python 'u başlatın ve Caffe2 içeri aktarın. <br/> * JupyterHub: [jupyterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve ardından örnek not defterlerini bulmak için Caffe2 dizinine gidin. Bazı Not defterleri, Python kodunda Caffe2 kökünün ayarlanmış olmasını gerektirir; /seçenek/caffe2yazın. |

## <a name="chainerhttpschainerorg"></a>[Chainer](https://chainer.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 5,2 |
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)     |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | Chainer, Python 3,5 ' ye yüklenir. |
| Nasıl çalıştırılır?      | Terminal: Python 3,5 ortamını etkinleştirin, `python` ' ı çalıştırın ve ardından-1 @no__t. <br/> * JupyterHub: [jupyterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)ve ardından örnek not defterlerini bulmak Için Chainer dizinine gidin.| 

## <a name="cuda-cudnn-nvidia-driverhttpsdevelopernvidiacomcuda-toolkit"></a>[CUDA, cuDNN, NVıDıA sürücüsü](https://developer.nvidia.com/cuda-toolkit)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 10.0.130|
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| DSVM 'de nasıl yapılandırılır/yüklenir?  |_NVIDIA-SMI_ sistem yolunda kullanılabilir.  |
| Nasıl çalıştırılır?      | Bir komut istemi (Windows üzerinde) veya bir Terminal (Linux 'ta) açın ve ardından _NVIDIA-SMI_' yi çalıştırın. |


## <a name="horovodhttpsgithubcomuberhorovod"></a>[Horovod](https://github.com/uber/horovod)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 0.16.1|
| Desteklenen DSVM sürümleri      | Linux (Ubuntu)   |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | Horovod, Python 3,5 ' de yüklü |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python 'u çalıştırın. |

## <a name="kerashttpskerasio"></a>[Keras](https://keras.io/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 2.2.4 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | Keras, Python 3,6 ' ye Windows üzerinde ve Linux 'ta Python 3,5 ' de yüklendi |
| Nasıl çalıştırılır?      | Terminalde doğru ortamı etkinleştirin ve Python 'u çalıştırın. |

## <a name="microsoft-cognitive-toolkit-cntkhttpsdocsmicrosoftcomcognitive-toolkit"></a>[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 2.5.1 |
| Desteklenen DSVM sürümleri      | Windows ve Linux   |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | CNTK, [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) ' de Python 3,6 ' de ve [Linux](./dsvm-languages.md#python-linux-edition)üzerinde Python 3,5 ' de yüklüdür) |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve Python 'U çalıştırın. <br/>Jupyter: [Jupyter](provision-vm.md) veya [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)'a bağlanın ve ardından örnekler için cntk dizinini açın. |

## <a name="pytorchhttpspytorchorg"></a>[PyTorch](https://pytorch.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.2.0 |
| Desteklenen DSVM sürümleri      | Linux |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | [Python 3,5](dsvm-languages.md#python-linux-edition)' de yüklü. Örnek Jupkiter Not defterleri dahil edilmiştir ve örnekler/dsvm/Samples/pytorch. |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın.<br/>* [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): bağlanın ve ardından örnekler Için PyTorch dizinini açın.  |

## <a name="tensorflowhttpswwwtensorfloworg"></a>[TensorFlow](https://www.tensorflow.org/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1,13 |
| Desteklenen DSVM sürümleri      | Windows, Linux |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) üzerinde [Linux](dsvm-languages.md#python-linux-edition) ve Python 3,6 ' de Python 3,5 ' ye yüklendi |
| Nasıl çalıştırılır?      | Terminal: doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın. <br/> * Jupyter: [Jupyter](provision-vm.md) veya [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)'a bağlanın ve örnekler için TensorFlow dizinini açın.   |

## <a name="tensorflow-servinghttpswwwtensorfloworgserving"></a>[TensorFlow hizmeti](https://www.tensorflow.org/serving/)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1,12 |
| Desteklenen DSVM sürümleri      | Linux |
| DSVM 'de nasıl yapılandırılır/yüklenir?  | tensorflow_model_server, terminalde mevcuttur. |
| Nasıl çalıştırılır?      |  Örnekler [çevrimiçi](https://www.tensorflow.org/serving/)olarak kullanılabilir.   |


## <a name="theanohttpsgithubcomtheanotheano"></a>[Theano](https://github.com/Theano/Theano)

|    |           |
| ------------- | ------------- |
| Desteklenen sürüm (ler) | 1.0.3 |
| Desteklenen DSVM sürümleri      | Linux |
| DSVM 'de nasıl yapılandırılır/yüklenir?  |Bu, Python 2,7 ' de (_root_) ve Python 3,5 (_py35_) ortamında yüklenir. |
| Nasıl çalıştırılır?      |  Terminal: istediğiniz Python sürümünü etkinleştirin (root veya py35), Python 'u çalıştırın ve ardından Ano içeri aktarın.<br/>* Jupyter: Python 2,7 veya 3,5 çekirdeğini seçin ve sonra da Ano içeri aktarın.  <br/>Son matematik çekirdek kitaplığı (MKL) hatasını geçici olarak çözmek için öncelikle MKL iş parçacığı katmanını aşağıdaki şekilde ayarlamanız gerekir:<br/><br/>`export MKL_THREADING_LAYER=GNU`  |