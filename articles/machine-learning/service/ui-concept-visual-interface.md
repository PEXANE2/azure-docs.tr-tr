---
title: Görsel arabirim
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için görsel arabirimi (Önizleme) oluşturan hüküm, kavramlar ve iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.date: 9/23/2019
ms.openlocfilehash: a23f123c6ffadaad4f830e1f4eab3820e6ef56f6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692215"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning"></a>Azure Machine Learning için görsel arabirim nedir? 

Azure Machine Learning için görsel arabirim (Önizleme), kod yazmadan, verileri dağıtmanıza, eğitmenize, test etmenize, dağıtmanıza, yönetmenize ve izlemenize olanak sağlar.

Programlama gerekmez, modelinizi oluşturmak için [veri kümelerini](#datasets) ve [modülleri](#module) görsel olarak bağlamanız gerekir.

Görsel arabirim, Azure Machine Learning [çalışma alanınızı](concept-workspace.md) şu amaçlarla kullanır:

+ Çalışma alanında işlem [hatları](#pipeline) oluşturun, düzenleyin ve çalıştırın.
+ [Veri kümelerine](#datasets)erişin.
+ İşlem hattını çalıştırmak için çalışma alanındaki [işlem kaynaklarını](#compute) kullanın. 
+ [Modelleri](concept-azure-machine-learning-architecture.md#models)kaydedin.
+ İşlem hatlarını REST uç noktaları olarak [yayımlayın](#publish) .
+ Modelleri, çalışma alanındaki bilgi işlem kaynaklarında ardışık düzen uç noktaları (toplu çıkarım için) veya gerçek zamanlı uç noktalar olarak [dağıtın](#deployment) .

![Görsel arabirime genel bakış](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>İş Akışı

Görsel arabirim, bir modeli hızlıca oluşturmak, test etmek ve yinelemek için size etkileşimli ve görsel bir tuval sağlar. 

+ [Veri kümelerini](#datasets) ve [modülleri](#module) tuval üzerine sürükleyip bırakın.
+ Bir işlem [hattı](#pipeline)oluşturmak için modülleri birbirine bağlayın.
+ Machine Learning hizmet çalışma alanının işlem kaynağını kullanarak ardışık düzeni çalıştırın.
+ İşlem hattını düzenleyerek ve yeniden çalıştırarak model tasarımınızda yineleme yapın.
+ Hazırsanız, **eğitim** işlem hattınızı bir **çıkarım ardışık düzenine**dönüştürün.
+ İşlem hattınızı, kendisini oluşturduğu Python kodu olmadan yeniden göndermek istiyorsanız bir REST uç noktası olarak [yayımlayın](#publish) .
+ Modelinize başkaları tarafından erişilebilmesi için, çıkarım ardışık düzenini bir ardışık düzen uç noktası veya gerçek zamanlı uç nokta olarak [dağıtın](#deployment) .

## <a name="pipeline"></a>İşlem hattı

Sıfırdan bir ML işlem [hattı](concept-azure-machine-learning-architecture.md#ml-pipelines) oluşturun veya var olan bir örnek işlem hattını şablon olarak kullanın. İşlem hattını her çalıştırdığınızda, yapıtlar çalışma alanınızda depolanır. İşlem hattı çalıştırmaları [denemeleri](concept-azure-machine-learning-architecture.md#experiments)olarak gruplandırılır.

İşlem hattı, bir model oluşturmak için birlikte bağlandığınız veri kümeleri ve analitik modüllerden oluşur. Özel olarak, geçerli bir işlem hattı şu özelliklere sahiptir:

* Veri kümeleri yalnızca modüllere bağlı olabilir.
* Modüller, veri kümelerine ya da diğer modüllere bağlı olabilir.
* Modüller için tüm giriş bağlantı noktalarında veri akışına bir bağlantı bulunmalıdır.
* Her modül için gerekli tüm parametrelerin ayarlanması gerekir.


Görsel arabirime nasıl başlacağınızı öğrenmek için bkz. [öğretici: görsel arabirim ile otomobil fiyatını tahmin](ui-tutorial-automobile-price-train-score.md)etme.

## <a name="datasets"></a>Veri kümeleri

Machine Learning veri kümesi, verilerinize erişmeyi ve bunlarla çalışmayı kolaylaştırır. Bir dizi örnek veri kümesi, denemeniz için görsel arabirime dahildir. Gerektiğinde daha [fazla veri kümesi kaydedebilirsiniz.](./how-to-create-register-datasets.md)

## <a name="module"></a>Modül

Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Görsel arabirimde, veri giriş işlevlerinden eğitim, Puanlama ve doğrulama işlemlerine kadar birçok modül bulunur.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki Özellikler bölmesinde görüntülenir. Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

![Modül özellikleri](media/ui-concept-visual-interface/properties.png)

Kullanılabilir makine öğrenimi algoritmalarının kitaplığı ile ilgili bazı yardım için bkz. [algoritma & modül başvurusuna genel bakış](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>İşlem kaynakları

İş hattınızı çalıştırmak ve dağıtılan modellerinizi gerçek zamanlı uç noktalar veya ardışık düzen uç noktaları olarak barındırmak (Batch çıkarımı için) için çalışma alanınızdan işlem kaynaklarını kullanın. Desteklenen işlem hedefleri şunlardır:

| İşlem hedefi | Eğitim | Kurulum |
| ---- |:----:|:----:|
| Azure Machine Learning işlem | ✓ | |
| Azure Kubernetes Service | | ✓ |

İşlem hedefleri Machine Learning [çalışma alanınıza](concept-workspace.md)iliştirilir. İşlem hedeflerinizi [Azure Portal](https://portal.azure.com) çalışma alanınızda veya [çalışma alanınızın giriş sayfasında (Önizleme)](https://ml.azure.com)yönetirsiniz.

## <a name="publish"></a>Yayımlama

İşlem hattınızı hazırladıktan sonra, bunu bir REST uç noktası olarak yayımlayabilirsiniz. Bir [Publishedpipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) , kendisini oluşturan Python kodu olmadan gönderilebilir.

Ayrıca, bir PublishedPipeline, farklı PipelineParameter değerleri ve girişleri ile bir işlem hattını yeniden göndermek için kullanılabilir.

## <a name="deployment"></a>Kurulum

Tahmine dayalı modeliniz hazırlandıktan sonra, görsel arabiriminden bir işlem hattı uç noktası veya gerçek zamanlı uç nokta olarak dağıtın.

Ardışık düzen uç noktası, farklı PipelineParameter değerleri ve toplu iş çıkarımı girişleri ile bir işlem hattı çalıştırması gönderebileceğiniz bir [PublishedPipeline.

Gerçek zamanlı uç nokta, bir uygulama ile Puanlama modeliniz arasında bir arabirim sağlar. Bir dış uygulama, Puanlama modeliyle gerçek zamanlı olarak iletişim kurabilir. Gerçek zamanlı bir uç noktaya yapılan çağrı, bir dış uygulamaya tahmin sonuçları döndürür. Gerçek zamanlı bir uç noktaya çağrı yapmak için, uç noktasını dağıtırken oluşturulan bir API anahtarı geçirirsiniz. Uç nokta, Web programlama projeleri için popüler bir mimari seçimi olan REST 'i temel alır.

Modelinizi dağıtmayı öğrenmek için bkz. [öğretici: Visual arabirimiyle makine öğrenimi modeli dağıtma](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Sonraki adımlar

* Tahmine dayalı analiz ve makine öğrenimi hakkında [öğreticiyi öğrenin: görsel arabirimle otomobil fiyatını tahmin](ui-tutorial-automobile-price-train-score.md) edin
* Örneklerden birini kullanın ve gereksinimlerinize göre değiştirin:

    * [Örnek 1-gerileme: tahmin fiyatı](how-to-ui-sample-regression-predict-automobile-price-basic.md)
    * [Örnek 2-gerileme: fiyat ve karşılaştırma algoritmalarını tahmin etme](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Örnek 3-sınıflandırma: kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
    * [Örnek 4-sınıflandırma: kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Örnek 5-sınıflandırma: dalgalanma, uygulama ve güncel satışı tahmin etme](how-to-ui-sample-classification-predict-churn.md)
    * [Örnek 6-sınıflandırma: uçuş gecikmelerini tahmin etme](how-to-ui-sample-classification-predict-flight-delay.md)

