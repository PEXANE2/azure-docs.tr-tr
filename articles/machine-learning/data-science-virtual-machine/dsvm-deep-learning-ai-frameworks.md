---
title: Ayrıntılı öğrenme & AI çerçeveleri
titleSuffix: Azure Data Science Virtual Machines
description: TensorFlow, PyTorch, keras, Caffe, MXNet, Horovod, Theano, Chainer gibi Azure Veri Bilimi Sanal Makinesi derin öğrenme çerçeveleri ve araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/11/2019
ms.author: vijetaj
ms.openlocfilehash: 3c9510b99bbdc7db292ecdc0bf59aadc305cf0c8
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035833"
---
# <a name="deep-learning-and-ai-frameworks"></a>Derin Öğrenme ve AI çerçeveleri
[Veri bilimi sanal makinesi](https://aka.ms/dsvm) (dsvm), tahmine dayalı analiz ve görüntü ve dil anlama gibi bilişsel Özellikler Ile yapay zeka (AI) uygulamaları oluşturmaya yardımcı olacak çok sayıda derin öğrenme çerçevesini destekler.

DSVM 'de bulunan derin öğrenme çerçeveleri şunları içerir:

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

|&nbsp;Dsvm&nbsp;'de&nbsp;DL araçları|Windows|Linux|Kullanım&nbsp;notları|
|---------|:-------------------:|:------------------:|-----|
|[TensorFlow](https://www.tensorflow.org/) | Evet (Windows 2016) | Evet |[Linux ve windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de Python 3,5 ' ye ve [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition)' de Python 3,6 ' ye yüklenmiştir. DSVM 'ye örnek bir Jupyıter Not defteri dahildir.<br/><br/>**Çalıştırmak için**:<br/>* Terminal: doğru ortamı etkinleştirin ve Python 'u çalıştırın. <br/> Jupyter [Jupi](provision-vm.md#tools) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve örnekler için TensorFlow dizinini açın.  |
|[PyTorch](https://pytorch.org/)| Hayır | Evet |[Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition)' de yüklü. Örnek Jupkiter Not defterleri dahil edilmiştir ve örnekler/dsvm/Samples/pytorch.    <br/><br/>**Çalıştırmak için**<br/>* Terminal: doğru ortamı etkinleştirin ve Python 'u çalıştırın.<br/>* [Jupyterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux): Ardından, örnekler için PyTorch dizinini bağlayın ve açın.  |
|[Keras](https://keras.io/)| Evet | Evet |API, [windows 2016](dsvm-languages.md#python-windows-server-2016-edition)üzerinde [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve python 3,6 üzerinde Python 3,5 ' ye yüklenir. [Örnekler](https://github.com/fchollet/keras/tree/master/examples)<br/><br/>**Çalıştırmak için**:<br/>* Terminal: doğru ortamı etkinleştirin ve Python 'u çalıştırın. <br/> Jupyter GitHub konumundan örnekleri indirin, [jupi](provision-vm.md#tools) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnek dizini açın. |
|[Caffe](https://github.com/caffe2/caffe2) | Hayır |Evet (Ubuntu)|Caffe yüklü `/opt/caffe`.   Örnekler içinde `/opt/caffe/examples`. <br/><br/>**Çalıştırmak Için**X2GO kullanarak sanal makinenizde oturum açın ve ardından yeni bir Terminal başlatın ve şunu girin:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Örnek Not Defterleri ile yeni bir tarayıcı penceresi açılır. İkili dosyaları içinde /opt/caffe/build/install/bin yüklenir.<br/><br/>Caffe'nın yüklü sürümü, Python 2.7 gerektirir ve varsayılan olarak etkinleştirilen Python 3.5 ile çalışmaz. Python 2,7 ' ye geçmek için, `source activate root` Anaconda ortamını switch ' i çalıştırın.|
|[Caffe2](https://github.com/caffe2/caffe2) | Hayır |Evet (Ubuntu)|Caffe2 yüklü [Python 2.7 (kök) conda ortam](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Kaynağı `/opt/caffe2`.<br/>Örnek Not Defterleri JupyterHub dahil edilir.<br/><br/>**Çalıştırmak için**:<br/>*, Terminal: etkinleştirme [kök Python ortamı](dsvm-languages.md#python-linux-and-windows-server-2012-edition), Python'ı başlatın ve caffe2 içeri aktarın. <br/> * Giriş JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek not defterleri bulmak için Caffe2 dizine gidin. Bazı dizüstü bilgisayarlar Python kodunda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin. |
|[Torch](http://torch.ch/) | Hayır |Evet (Ubuntu)|Torch yüklü `/dsvm/tools/torch`. PyTorch Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı. Torch örnekleri içinde `/dsvm/samples/torch` ve pytorch örnekleri içinde `/dsvm/samples/pytorch`bulunur. |
|[MXNet](https://mxnet.io/) | Evet (Windows 2016) | Evet|MXNet yüklü `C:\dsvm\tools\mxnet` Windows üzerinde ve `/dsvm/tools/mxnet` Linux üzerinde. Python bağlamaları yüklenen Python 3.5 [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R bağlamaları, Ubuntu'da da yüklenir.<br/><br/>Örnek Jupyter not defterleri bulunur. <br/><br/>**Çalıştırmak için**:<br/>Den Doğru ortamı etkinleştirin ve Python 'u çalıştırın. <br/> Jupyter [Jupi](provision-vm.md#tools) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın ve ardından örnek için mxnet dizinini açın.|
|[MXNet model sunucusu](https://github.com/awslabs/mxnet-model-server) | Hayır | Evet |MXNet ve ONNX modelleri için HTTP uç noktaları oluşturmak üzere bir sunucu. _mxnet modeli sunucu_ terminal bulunur. [Mxnet model sunucusu sayfasındaki](https://github.com/awslabs/mxnet-model-server)örnekler.|
|[Horovod](https://github.com/uber/horovod) | Hayır | Evet (Ubuntu) |TensorFlow için dağıtılmış derin öğrenme çerçevesi. Horovod üzerinde Python 3.5 yüklenir [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  [Örneklere bakın](https://github.com/uber/horovod/tree/master/examples)<br/><br/>**Çalıştırmak için**terminalde doğru ortamı etkinleştirin ve Python 'u çalıştırın. |
|[Theano](https://github.com/Theano/Theano) | Hayır | Evet (Ubuntu) |Theano Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.<br/><br/>**Çalıştırmak için**: <br/>Den İstediğiniz Python sürümünü (root veya py35) etkinleştirin, Python çalıştırın ve ardından Ano içeri aktarın.<br/>Jupyter Python 2,7 veya 3,5 çekirdeğini seçin, sonra da bir Ano içeri aktarın.  <br/>Son MKL hatanın geçici olarak çözmek için katman iş parçacığı MKL ayarlamanız gerekir:<br/><br/>_Export MKL_THREADING_LAYER = GNU_|
|[Chainer](https://chainer.org/) |Hayır | Evet |Bağlayıcı yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL ve ChainerCV da yüklenir. <br/><br/>Örnek Not Defterleri JupyterHub dahil edilir.<br/><br/>**Çalıştırmak için**: <br/>Den [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ortamını etkinleştirin, _Python_çalıştırın ve ardından Chainer alın. <br/> * JupyterHub: [Jupyıterhub 'A bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)ve ardından örnek not defterlerini bulmak Için Chainer dizinine gidin.|
|[NVIDIA rakamları](https://github.com/NVIDIA/DIGITS) | Hayır | Evet (Ubuntu) |Hızlı eğitim derinlemesine öğrenme modelleri için NVıDıA 'dan derin öğrenme sistemi. BASAMAK yüklü `/dsvm/tools/DIGITS` ve kullanılabilir bir hizmet olarak adlandırılan _basamak_.  <br/><br/>**Çalıştırmak için**: <br/>VM X2Go ile oturum açın. Bir terminalde, hizmeti ```sudo systemctl start digits```başlatın. <br/><br/>Hizmeti başlatmak için yaklaşık bir dakika sürer. Bir web tarayıcısını Başlat ve gidin `http://localhost:5000`. BASAMAK güvenli bir oturum açma sağlamaz ve VM dışında açıkta kalmamalıdır unutmayın.|
|[CUDA, cuDNN, NVıDıA sürücüsü](https://developer.nvidia.com/cuda-toolkit) |Evet | Evet | |
|NVIDIA SMI|Evet | Evet |GPU etkinliğini sorgulamak için NVıDıA aracı. _NVIDIA SMI_ sistemi yolu üzerinde kullanılabilir. <br/><br/>Bir komut istemi (Windows) veya bir terminalde (Linux) başlatın ve ardından çalıştırın _NVIDIA SMI_.|
|[TensorFlow hizmeti](https://www.tensorflow.org/serving/) | Hayır | Evet |Bir TensorFlow modelinde çıkarması için bir sunucu. _tensorflow_model_server_ terminal kullanılabilir. Örnekler mevcuttur [çevrimiçi](https://www.tensorflow.org/serving/).|
|[TensorRT](https://developer.nvidia.com/tensorrt) |  Hayır | Evet (Ubuntu) |Derin öğrenme NVIDIA çıkarımı sunucudan vm'si. TensorRT yüklü olarak bir _apt_ paket. Örnekler mevcuttur [çevrimiçi](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).|
|[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)|Evet | Evet | [Linux ve windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de Python 3,5 ' ye ve [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition)' de Python 3,6 ' ye yüklenmiştir. DSVM 'ye örnek bir Jupyıter Not defteri dahildir. <br/><br/>**Çalıştırmak için**: <br/>Den Doğru ortamı etkinleştirin ve Python 'u çalıştırın. <br/>Jupyter [Jupi](provision-vm.md#tools) veya [Jupyıterhub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)'a bağlanın, ardından örnekler için cntk dizinini açın. |
|Ayrıntılı su|Hayır | Evet (Ubuntu) |H2O için derin öğrenme çerçevesi, [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ' de derin su yüklenir ve ' `/dsvm/tools/deep_water`de de mevcuttur. Örnek Not Defterleri JupyterHub dahil edilir. Ayrıntılı su CUDA 8 cuDNN 5.1 ile gerektirir. Diğer derin öğrenme çerçeveleri CUDA 9 ve cuDNN 7 olarak bu varsayılan olarak, kitaplık yolu değil. CUDA 8 + cuDNN 5.1 için ayrıntılı su kullanmak için:<br/><br/>```export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}```<br/>```export CUDA_ROOT=/usr/local/cuda-8.0```<br/><br/>Ayrıntılı su kullanmak için:<br/>* Terminal: [python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ortamını etkinleştirin ve _Python_'u çalıştırın. <br/>* JupyterHub: [jupyterhub 'a bağlanın](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), sonra örnek not defterlerini bulmak için deep_water dizinine gidin.|
