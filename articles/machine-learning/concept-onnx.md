---
title: 'ONNX: yüksek perf, çapraz platform çıkarımı'
titleSuffix: Azure Machine Learning
description: Açık Sinir Selavar Değişimi'ni (ONNX) kullanmanın makine öğrenimi modelinizin çıkarımını optimize etmesine nasıl yardımcı olabileceğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: 98aebb4733c2aa2a6d0b0217f1f437bcea1992e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270179"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX ve Azure Machine Learning: ML modelleri oluşturun ve hızlandırın

Açık Sinir [Selavar Değişimi'ni](https://onnx.ai) (ONNX) kullanmanın makine öğrenimi modelinizin çıkarımını optimize etmesine nasıl yardımcı olabileceğini öğrenin. Çıkarım veya model puanlama, dağıtılan modelin tahmin için kullanıldığı aşamadır, en yaygın olarak üretim verilerinde. 

Donanım özelliklerinden en iyi şekilde yararlanmak için modeli ve çıkarım kitaplığını ayarlamanız gerektiğinden, çıkarım (veya model puanlama) için makine öğrenimi modellerini optimize etmek zordur. Her biri farklı yeteneklere ve özelliklere sahip olduğundan, farklı türde platformlarda (bulut/kenar, CPU/GPU, vb.) en iyi performansı elde etmek istiyorsanız sorun son derece zorlaşır. Çeşitli platformlarda çalışması gereken çeşitli çerçevelerden modeller varsa, karmaşıklık artar. Tüm farklı çerçeve ve donanım kombinasyonlarını optimize etmek çok zaman alır. Tercih ettiğiniz çerçevede bir kez eğitim vermek ve bulut veya kenarda herhangi bir yerde çalıştırmak için bir çözüm gereklidir. İŞTE ONNX burada devreye giriyor.

Microsoft ve iş ortaklarından oluşan bir topluluk, onnx'i makine öğrenimi modellerini temsil etmek için açık bir standart olarak oluşturdu. TensorFlow, PyTorch, SciKit-Learn, Keras, Chainer, MXNet ve MATLAB gibi [birçok çerçeveden](https://onnx.ai/supported-tools) modeller ihraç edilebilir veya standart ONNX biçimine dönüştürülebilir. Modeller ONNX formatında olduktan sonra, çeşitli platformlarda ve cihazlarda çalıştırılabilir.

[ONNX Runtime, ONNX](https://github.com/Microsoft/onnxruntime) modellerini üretime dağıtmak için yapılan yüksek performanslı bir çıkarım motorudur. Hem bulut hem de kenar için optimize edilmiş ve Linux, Windows ve Mac'te çalışır. C++'da yazılmış, c, python ve C# API'leri de vardır. ONNX Runtime, ONNX-ML belirtiminin tümü için destek sağlar ve NVidia GPU'larında TensorRT gibi farklı donanımlarda hızlandırıcılarla entegre edilir.

ONNX Runtime, Bing, Office ve Bilişsel Hizmetler gibi yüksek ölçekli Microsoft hizmetlerinde kullanılır. Performans kazançları bir dizi etkene bağlıdır, ancak bu Microsoft hizmetleri __CPU'da ortalama 2 kat performans artışı__elde etti. ONNX Runtime, yüz milyonlarca cihazda Windows ML'nin bir parçası olarak da kullanılır. Çalışma süresini Azure Machine Learning ile kullanabilirsiniz. ONNX Runtime'ı kullanarak kapsamlı üretim sınıfı optimizasyonlardan, testlerden ve devam eden iyileştirmelerden yararlanabilirsiniz.

[![Eğitimi, dönüştürücüleri ve dağıtımı gösteren ONNX akış diyagramı](./media/concept-onnx/onnx.png)](././media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>ONNX modellerini alın

ONNX modellerini çeşitli yollarla elde edebilirsiniz:
+ Azure Machine Learning'de yeni bir ONNX modeli öğrenin (bu makalenin altındaki örneklere bakın)
+ Varolan modeli başka bir biçimden ONNX'e dönüştürün [(eğitimlere](https://github.com/onnx/tutorials)bakın) 
+ [ONNX Model Hayvanat Bahçesi'nden](https://github.com/onnx/models) önceden eğitilmiş bir ONNX modeli alın (bu makalenin altındaki örneklere bakın)
+ [Azure Özel Görme hizmetinden](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) özelleştirilmiş bir ONNX modeli oluşturun 

Görüntü sınıflandırma, nesne algılama ve metin işleme gibi birçok model ONNX modelleri olarak temsil edilebilir. Ancak bazı modeller başarılı bir şekilde dönüştürülemeyebilir. Bu durumla karşı karşıya ysanız, lütfen kullandığınız ilgili dönüştürücünün GitHub'ına bir sorun gönderin. Sorun giderilene kadar varolan biçim modelinizi kullanmaya devam edebilirsiniz.

## <a name="deploy-onnx-models-in-azure"></a>ONNX modellerini Azure'da dağıtma

Azure Machine Learning ile ONNX modellerinizi dağıtabilir, yönetebilir ve izleyebilirsiniz. Standart [dağıtım iş akışını](concept-model-management-and-deployment.md) ve ONNX Runtime'ı kullanarak bulutta barındırılan bir REST bitiş noktası oluşturabilirsiniz. Kendiniz için denemek için bu makalenin sonunda örnek Jupyter dizüstü bakın. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Python ile ONNX Runtime'ı yükleyin ve kullanın

ONNX Runtime için Python paketleri [PyPi.org](https://pypi.org) [(CPU](https://pypi.org/project/onnxruntime), [GPU)](https://pypi.org/project/onnxruntime-gpu)mevcuttur. Lütfen kurulumdan önce [sistem gereksinimlerini](https://github.com/Microsoft/onnxruntime#system-requirements) okuyun. 

 Python için ONNX Runtime'ı yüklemek için aşağıdaki komutlardan birini kullanın: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
```

Python komut dosyanızda ONNX Runtime'ı aramak için şunları kullanın:    
```python
import onnxruntime
session = onnxruntime.InferenceSession("path to model")
```

Modele eşlik eden belgeler genellikle modeli kullanmak için giriş ve çıktıları söyler. Modeli görüntülemek için [Netron](https://github.com/lutzroeder/Netron) gibi bir görselleştirme aracı da kullanabilirsiniz. ONNX Runtime ayrıca model meta verilerini, girişlerini ve çıktılarını sorgulamanızı sağlar:    
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Modelinizi çıkarmak için, `run` döndürülen çıktılar listesinde kullanın ve geçirin (hepsini istiyorsanız boş bırakın) ve giriş değerlerinin bir haritasını kullanın. Sonuç çıktıların bir listesidir.  
```python
results = session.run(["output1", "output2"], {
                      "input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Python API başvurusu tamamlayın, [ONNX Runtime başvuru dokümanlarına](https://aka.ms/onnxruntime-python)bakın.    

## <a name="examples"></a>Örnekler

Örneğin ONNX modellerini oluşturan ve dağıtan dizüstü bilgisayarlar için [nasıl kullanılır-azureml/deployment/onnx'e](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) bakın.

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Daha fazla bilgi

ONNX hakkında daha fazla bilgi edinin veya projeye katkıda bulunun:
+ [ONNX proje web sitesi](https://onnx.ai)
+ [GitHub üzerinde ONNX kodu](https://github.com/onnx/onnx)

ONNX Runtime hakkında daha fazla bilgi edinin veya projeye katkıda bulunun:
+ [ONNX Çalışma Zamanı GitHub Repo](https://github.com/Microsoft/onnxruntime)


