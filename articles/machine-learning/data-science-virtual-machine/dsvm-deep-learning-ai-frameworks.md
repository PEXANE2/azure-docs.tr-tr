---
title: Ayrıntılı öğrenme & AI çerçeveleri
titleSuffix: Azure Data Science Virtual Machine
description: TensorFlow, PyTorch, keras, Caffe, MXNet, Horovod, Theano, Chainer gibi Azure Veri Bilimi Sanal Makinesi 'da sunulan derin öğrenme çerçeveleri ve araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 08/11/2019
ms.openlocfilehash: f71a2cc5c0a430037b429aac4f3ec4dc5f5f933d
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208113"
---
# <a name="deep-learning-and-ai-frameworks-for-azure-data-science-vm"></a>Azure Veri Bilimi VM'si için derin öğrenme ve AI çerçeveleri
[Veri bilimi sanal makinesi](https://aka.ms/dsvm) (dsvm), tahmine dayalı analiz ve görüntü ve dil anlama gibi bilişsel özellikler ile yapay zeka (AI) uygulamaları oluşturmaya yardımcı olacak çok sayıda derin öğrenme çerçevesini destekler.

DSVM aracılığıyla kullanılabilen derin öğrenme çerçeveleri şunlardır:

+ TensorFlow
+ PyTorch
+ Keras
+ Caffe
+ Caffe2
+ Torch
+ MXNet modeli sunucusu
+ MXNet
+ Horovod
+ Theano
+ Chainer
+ Ayrıntılı su
+ NVIDIA BASAMAK
+ NVIDIA SMI
+ TensorFlow sunma
+ TensorRT
+ Microsoft Bilişsel Araç Seti

|Dsvm 'de&nbsp;&nbsp;derinöğrenme&nbsp;araçları|Windows|Linux|Kullanım&nbsp;notları|
|---------|-------------------|------------------|-----|
|[TensorFlow](https://www.tensorflow.org/) | Evet (Windows 2016) | Evet |[Linux ve windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de Python 3,5 ' ye ve [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition)' de Python 3,6 ' ye yüklenmiştir. DSVM 'ye örnek bir Jupyıter Not defteri dahildir.<br/><br/>**Çalıştırmak için**:<br/>Den Doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın. <br/> Jupyter [Jupi](provision-vm.md) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnekler için TensorFlow dizinini açın.  |
|[PyTorch](https://pytorch.org/)| Hayır | Evet |[Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition)' de yüklü. Örnek Jupkiter Not defterleri dahil edilmiştir ve örnekler/dsvm/Samples/pytorch.    <br/><br/>**Çalıştırmak için**:<br/>Den Doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın.<br/>* [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux): Daha sonra örnekler için PyTorch dizinini bağlayın ve açın.  |
|[Keras](https://keras.io/)| Evet | Evet |API, [Linux ve windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de Python 3,5 ' ye ve [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition)' te Python 3,6 ' ye yüklenir. [Örneklere bakın](https://github.com/fchollet/keras/tree/master/examples).<br/><br/>**Çalıştırmak için**:<br/>Den Doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın. <br/> Jupyter GitHub konumundan örnekleri indirin, [jupi](provision-vm.md) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnek dizini açın. |
|[Caffe](https://github.com/caffe2/caffe2) | Hayır |Evet (Ubuntu)|Caffe yüklü `/opt/caffe`.   Örnekler içinde `/opt/caffe/examples`. <br/><br/>**Çalıştırmak için**, X2GO kullanarak sanal makinenizde oturum açın ve ardından yeni bir Terminal başlatın ve aşağıdakileri girin:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Örnek Not Defterleri ile yeni bir tarayıcı penceresi açılır. İkili dosyaları içinde /opt/caffe/build/install/bin yüklenir.<br/><br/>Caffe 'nin yüklü sürümü Python 2,7 gerektirir ve varsayılan olarak etkinleştirilen Python 3,5 ile çalışmaz. Python 2,7 ' ye geçmek için, `source activate root` Anaconda ortamına geçiş yapmak üzere öğesini çalıştırın.|
|[Caffe2](https://github.com/caffe2/caffe2) | Hayır |Evet (Ubuntu)|Caffe2 yüklü [Python 2.7 (kök) conda ortam](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Kaynağı `/opt/caffe2`.<br/>Örnek Not Defterleri JupyterHub dahil edilir.<br/><br/>**Çalıştırmak için**:<br/>Den [Kök Python ortamını](dsvm-languages.md#python-linux-and-windows-server-2012-edition)etkinleştirin, Python 'u başlatın ve Caffe2 içeri aktarın. <br/> * JupyterHub: [Jupyıterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)ve ardından örnek not defterlerini bulmak için Caffe2 dizinine gidin. Bazı dizüstü bilgisayarlar Python kodunda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin. |
|[Torch](http://torch.ch/) | Hayır |Evet (Ubuntu)|Torch yüklü `/dsvm/tools/torch`. PyTorch Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı. Torch örnekleri içinde `/dsvm/samples/torch` ve pytorch örnekleri içinde `/dsvm/samples/pytorch`bulunur. |
|[MXNet](https://mxnet.io/) | Evet (Windows 2016) | Evet|MXNet yüklü `C:\dsvm\tools\mxnet` Windows üzerinde ve `/dsvm/tools/mxnet` Linux üzerinde. Python bağlamaları yüklenen Python 3.5 [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R bağlamaları, Ubuntu'da da yüklenir.<br/><br/>Örnek Jupyter not defterleri bulunur. <br/><br/>**Çalıştırmak için**:<br/>Den Doğru ortamı etkinleştirin ve sonra Python 'u çalıştırın. <br/> Jupyter [Jupi](provision-vm.md) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnek için mxnet dizinini açın.|
|[MXNet model sunucusu](https://github.com/awslabs/mxnet-model-server) | Hayır | Evet |MXNet ve ONNX modelleri için HTTP uç noktaları oluşturmak üzere bir sunucu. _Mxnet-model-Server_ terminalde mevcuttur. [Mxnet model sunucusu sayfasındaki](https://github.com/awslabs/mxnet-model-server)örnekler.|
|[Horovod](https://github.com/uber/horovod) | Hayır | Evet (Ubuntu) |TensorFlow için dağıtılmış derin öğrenme çerçevesi. Horovod üzerinde Python 3.5 yüklenir [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  [Örneklere bakın](https://github.com/uber/horovod/tree/master/examples).<br/><br/>**Çalıştırmak için**terminalde doğru ortamı etkinleştirin ve Python ' ı çalıştırın. |
|[Theano](https://github.com/Theano/Theano) | Hayır | Evet (Ubuntu) |Bu, Python 2,7 ' de (_root_) ve Python 3,5 (_py35_) ortamında yüklenir.<br/><br/>**Çalıştırmak için**: <br/>Den İstediğiniz Python sürümünü (root veya py35) etkinleştirin, Python ' ı çalıştırın ve ardından Ano içeri aktarın.<br/>Jupyter Python 2,7 veya 3,5 çekirdeğini seçin ve sonra da bir Ano içeri aktarın.  <br/>Son matematik çekirdek kitaplığı (MKL) hatasını geçici olarak çözmek için öncelikle MKL iş parçacığı katmanını aşağıdaki şekilde ayarlamanız gerekir:<br/><br/>`export MKL_THREADING_LAYER=GNU`|
|[Chainer](https://chainer.org/) |Hayır | Evet |Bağlayıcı yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL ve ChainerCV da yüklenir. <br/><br/>Örnek Not Defterleri JupyterHub dahil edilir.<br/><br/>**Çalıştırmak için**: <br/>Den [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ortamını etkinleştirin, _Python_çalıştırın ve ardından Chainer öğesini içeri aktarın. <br/> * JupyterHub: [Jupyıterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)ve ardından örnek not defterlerini bulmak Için Chainer dizinine gidin.|
|[NVIDIA rakamları](https://github.com/NVIDIA/DIGITS) | Hayır | Evet (Ubuntu) |Daha hızlı eğitim için NVıDıA 'dan derin öğrenme sistemi. Basamaklar ' de `/dsvm/tools/DIGITS` yüklüdür ve _basamaklar_adlı bir hizmet olarak kullanılabilir.  <br/><br/>**Çalıştırmak için**: <br/>X2Go ile VM 'de oturum açın. Bir terminalde çalıştırarak ```sudo systemctl start digits```hizmeti başlatın. <br/><br/>Hizmeti başlatmak için yaklaşık bir dakika sürer. Bir Web tarayıcısı açın ve adresine gidin `http://localhost:5000`. BASAMAK güvenli bir oturum açma sağlamaz ve VM dışında açıkta kalmamalıdır unutmayın.|
|[CUDA, cuDNN, NVıDıA sürücüsü](https://developer.nvidia.com/cuda-toolkit) |Evet | Evet | |
|NVIDIA SMI|Evet | Evet |GPU etkinliğini sorgulamak için NVıDıA aracı; _NVIDIA-SMI_ sistem yolunda kullanılabilir. <br/><br/>Bir komut istemi (Windows üzerinde) veya bir Terminal (Linux 'ta) açın ve ardından _NVIDIA-SMI_' yi çalıştırın.|
|[TensorFlow hizmeti](https://www.tensorflow.org/serving/) | Hayır | Evet |Bir TensorFlow modelinde çıkarması için bir sunucu; tensorflow_model_server, terminalde mevcuttur. Örnekler mevcuttur [çevrimiçi](https://www.tensorflow.org/serving/).|
|[TensorRT](https://developer.nvidia.com/tensorrt) |  Hayır | Evet (Ubuntu) |NVıDıA 'dan derin öğrenme çıkarım sunucusu. TensorRT yüklü olarak bir _apt_ paket. Örnekler mevcuttur [çevrimiçi](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).|
|[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)|Evet | Evet | [Linux ve windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de Python 3,5 ' ye ve [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition)' de Python 3,6 ' ye yüklenmiştir. DSVM 'ye örnek bir Jupyıter Not defteri dahildir. <br/><br/>**Çalıştırmak için**: <br/>Den Doğru ortamı etkinleştirin ve Python 'u çalıştırın. <br/>Jupyter [Jupi](provision-vm.md) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnekler için cntk dizinini açın. |
|Ayrıntılı su|Hayır | Evet (Ubuntu) |H2O için derin öğrenme çerçevesi, [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de derin su yüklenir ve ' `/dsvm/tools/deep_water`de de mevcuttur. Örnek Not Defterleri JupyterHub dahil edilir. Ayrıntılı su CUDA 8 cuDNN 5.1 ile gerektirir. Bu, varsayılan olarak kitaplık yolunda değildir, çünkü diğer derin öğrenme çerçeveleri CUDA 9 ve cuDNN 7 ' yi kullanır. Kullanmak için CUDA 8 + cuDNN 5,1 ' i derin su için yüklemelisiniz:<br/><br/>```export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}```<br/>```export CUDA_ROOT=/usr/local/cuda-8.0```<br/><br/>Ayrıntılı su kullanmak için:<br/>Den [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ortamını etkinleştirin ve _Python_'u çalıştırın. <br/>* JupyterHub: [Jupyıterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)ve ardından örnek not defterlerini bulmak için deep_water dizinine gidin.|
