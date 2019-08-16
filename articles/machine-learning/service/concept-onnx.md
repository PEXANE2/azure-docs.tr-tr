---
title: ONNX ile yüksek performans, platformlar arası çıkarım
titleSuffix: Azure Machine Learning service
description: Hızlandırma modelleri için ONNX ve ONNX çalışma zamanı hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: 1d97e2d2698c482b75f037dbd8cde1027c472125
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534890"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX ve Azure Machine Learning: ML modellerini oluşturma ve hızlandırma

[Open sinir Network Exchange](https://onnx.ai) (onnx) kullanmanın, makine öğrenimi modelinizin çıkarımını en üst düzeye getirmenize nasıl yardımcı olabileceğini öğrenin. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır. 

Donanım yeteneklerini en iyi hale getirmek için modeli ve çıkarım kitaplığını ayarlamanız gerektiğinden, bu yana, makine öğrenimi modellerini çıkarma (veya model Puanlama) için optimize etmek zordur. Farklı türlerde platformlar (bulut/kenar, CPU/GPU vb.) üzerinde en iyi performansı almak istiyorsanız sorun son derece zor olur. çünkü her biri farklı özelliklere ve özelliklere sahiptir. Çeşitli platformlarda çalışması gereken çeşitli çerçeveler için modelleriniz varsa karmaşıklık artar. Tüm farklı çerçeve ve donanım birleşimlerini iyileştirmek çok zaman alır. Tercih ettiğiniz çerçevede bir kez eğitme ve bulutta veya kenarda her yerde çalıştırılan bir çözüm. Burada ONNX yer verilir.

Microsoft ve makine öğrenimi modellerini temsil etmek için açık bir standart olarak ONNX tarafından oluşturulan bir iş ortağı topluluğu. TensorFlow, PyTorch, SciKit-öğren, keras, Chainer, MXNet ve MATLAB gibi [birçok çerçeve](https://onnx.ai/supported-tools) modeli, standart onnx biçimine aktarılabilir veya dönüştürülebilirler. Modeller ONNX biçiminde olduktan sonra çeşitli platformlar ve cihazlarda çalıştırılabilir.

[Onnx çalışma zamanı](https://github.com/Microsoft/onnxruntime) , üretim için onnx modellerini dağıtmaya yönelik yüksek performanslı bir çıkarım altyapısıdır. Bulut ve kenar için en iyi duruma getirilmiştir ve Linux, Windows ve Mac üzerinde çalışmaktadır. İçinde C++yazılmış, C, Python ve C# API 'leri de vardır. ONNX çalışma zamanı tüm ONNX-ML belirtiminin desteğini sağlar ve ayrıca NVIDIA GPU 'Larda TensorRT gibi farklı donanımlarda Hızlandırıcılar ile tümleşir.

ONNX çalışma zamanı, Bing, Office ve bilişsel hizmetler gibi yüksek ölçekli Microsoft hizmetlerinde kullanılır. Performans kazançları bir dizi etkene bağlıdır, ancak bu Microsoft Hizmetleri __CPU üzerinde ortalama bir 2x performans kazancı__gördük. ONNX çalışma zamanı, yüzlerce milyonlarca cihazda Windows ML 'nin bir parçası olarak da kullanılır. Çalışma zamanını Azure Machine Learning hizmetleriyle birlikte kullanabilirsiniz. ONNX çalışma zamanını kullanarak, kapsamlı üretim sınıfı iyileştirmeleri, test ve devam eden geliştirmelerden yararlanabilirsiniz.

[![Eğitim, dönüştürücüler ve dağıtımı gösteren ONNX akış diyagramı](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>ONNX modelleri Al

Çeşitli şekillerde ONNX modelleri elde edebilirsiniz:
+ Azure Machine Learning hizmetinde yeni bir ONNX modeli eğitme (Bu makalenin altındaki örneklere bakın)
+ Varolan modeli başka bir biçimden ONNX 'e Dönüştür ( [öğreticilere](https://github.com/onnx/tutorials)bakın) 
+ [Onnx model Zoo](https://github.com/onnx/models) (Bu makalenin altındaki örneklere bakın) için önceden eğitilen BIR onnx modeli alın
+ Özelleştirilmiş ONNX modelden üreten [Azure özel görüntü işleme hizmeti](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 

Görüntü sınıflandırması, nesne algılama ve metin işleme gibi birçok model ONNX modelleri olarak temsil edilebilir. Ancak bazı modeller başarıyla dönüştürülemeyebilir. Bu durumla karşılaşırsanız lütfen kullandığınız ilgili dönüştürücünün GitHub 'da bir sorun bildirin. Sorun giderilene kadar mevcut biçim modelinizi kullanmaya devam edebilirsiniz.

## <a name="deploy-onnx-models-in-azure"></a>Azure'da ONNX modelleri dağıtma

Azure Machine Learning hizmeti ile dağıtma, yönetme ve ONNX modellerinizle izleyin. Standart [dağıtımı iş akışı](concept-model-management-and-deployment.md) ve ONNX çalışma zamanı, bulutta barındırılan bir REST uç noktası oluşturabilirsiniz. Kendiniz denemek için bu makalenin sonundaki örnek jupi not defterlerine bakın. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Python ile ONNX çalışma zamanını yükleyip kullanma

ONNX çalışma zamanına yönelik Python paketleri [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)) üzerinde bulunabilir. Lütfen yüklemeden önce [sistem gereksinimlerini](https://github.com/Microsoft/onnxruntime#system-requirements) okuyun. 

 Python için ONNX çalışma zamanını yüklemek için aşağıdaki komutlardan birini kullanın: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
```

Python betiğinizde ONNX çalışma zamanı çağırmak için kullanın:    
```python
import onnxruntime
session = onnxruntime.InferenceSession("path to model")
```

Model genellikle eşlik eden belgeleri giriş ve çıkışları için modeli kullanarak söyler. Bir görselleştirme aracı gibi kullanabilirsiniz [Netron](https://github.com/lutzroeder/Netron) modeli görüntülemek için. ONNX çalışma zamanı ayrıca sorgu model meta verilerini, girdileri ve çıktıları sağlar:    
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Çıkarım için kullanım modelinizi `run` ve döndürülen istediğiniz çıkışları (tümünün istiyorsanız bırakın boş) ve giriş değerleri haritasını listesinde geçirin. Sonuç çıktısı listesidir.  
```python
results = session.run(["output1", "output2"], {
                      "input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Tam Python API Başvurusu için bkz. [ONNX çalışma zamanı başvuru belgeleri](https://aka.ms/onnxruntime-python).    

## <a name="examples"></a>Örnekler

Bkz: [Yardım-How-to-kullanın-azureml/dağıtım/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) örneğin ONNX modelleri oluşturup Not.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Daha fazla bilgi

ONNX hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX proje Web sitesi](https://onnx.ai)
+ [Github'da ONNX kod](https://github.com/onnx/onnx)

ONNX çalışma zamanı hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX çalışma zamanı GitHub deposu](https://github.com/Microsoft/onnxruntime)


