---
title: Tasarımcı ile ML modelleri derleme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için tasarımcıyı oluşturan hüküm, kavramlar ve iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: peterlu
author: peterclu
ms.date: 11/12/2019
ms.openlocfilehash: efc9d7fb31dc9f63780c3f94238bbbfb17756089
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74973637"
---
# <a name="what-is-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcı (Önizleme) nedir? 
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Azure Machine Learning tasarımcı, makine öğrenimi modelleri oluşturmak için etkileşimli bir tuvalde [veri kümelerini](#datasets) ve [modülleri](#module) görsel olarak bağlamanıza olanak tanır. Tasarımcı ile çalışmaya başlama hakkında bilgi edinmek için bkz [. Öğretici: tasarımcı ile otomobil fiyatını tahmin](tutorial-designer-automobile-price-train-score.md) etme

![Azure Machine Learning tasarımcı örneği](./media/concept-ml-pipelines/designer-drag-and-drop.gif)

Tasarımcı, şu gibi paylaşılan kaynakları düzenlemek için Azure Machine Learning [çalışma alanınızı](concept-workspace.md) kullanır:

+ [İşlem hatları](#pipeline)
+ [Veri kümeleri](#datasets)
+ [İşlem kaynakları](#compute)
+ [Kayıtlı modeller](concept-azure-machine-learning-architecture.md#models)
+ [Yayınlanan işlem hatları](#publish)
+ [Gerçek zamanlı uç noktalar](#deploy)

## <a name="model-training-and-deployment"></a>Model eğitimi ve dağıtımı

Tasarımcı, makine öğrenimi modellerini derlemek, test etmek ve dağıtmak için size görsel bir tuval sağlar. Tasarımcı ile şunları yapabilirsiniz:

+ [Veri kümelerini](#datasets) ve [modülleri](#module) tuval üzerine sürükleyip bırakın.
+ Bir işlem [hattı taslağı](#pipeline-draft)oluşturmak için modülleri birbirine bağlayın.
+ Azure Machine Learning çalışma alanınızdaki işlem kaynaklarını kullanarak bir işlem [hattı çalıştırması](#pipeline-run) gönderebilirsiniz.
+ **Eğitim** işlem hatlarınızı **çıkarım**işlem hatlarına dönüştürün.
+ Farklı parametrelerle ve veri kümeleriyle yeni işlem hattı çalıştırmaları göndermek için işlem hatlarınızı bir REST **ardışık düzen uç noktasına** [yayımlayın](#publish) .
    + Parametreleri ve veri kümelerini değiştirirken birden çok modeli eğitmek için tek bir işlem hattını yeniden kullanmak üzere bir **eğitim işlem hattı** yayımlayın.
    + Daha önce eğitilen bir modeli kullanarak yeni verilerde tahmine dayalı hale getirmek için bir **toplu çıkarım işlem hattı** yayımlayın.
+ Gerçek zamanlı bir zaman noktasına gerçek zamanlı bir **çıkarım işlem hattı** [dağıtın](#deploy) ve yeni verilerde gerçek zamanlı olarak tahmine dayalı hale getirin.

![Tasarımcıda eğitim, toplu çıkarım ve gerçek zamanlı çıkarım için iş akışı diyagramı](media/concept-designer/designer-workflow-diagram.png)

## <a name="pipeline"></a>İşlem hattı

İşlem [hattı](concept-azure-machine-learning-architecture.md#ml-pipelines) , birlikte bağlandığınız veri kümeleri ve analitik modüllerden oluşur. İşlem hatları birçok kullanımı vardır: tek bir modeli gösteren bir işlem hattını veya birden çok modeli gösteren bir işlem hattı yapabilirsiniz. Gerçek zamanlı veya toplu olarak tahmin yapan bir işlem hattı oluşturabilir veya yalnızca verileri temizlemeleri için bir işlem hattı oluşturabilirsiniz. İşlem hatları, işinizi yeniden kullanmanıza ve projelerinizi düzenlemenize olanak tanır.

### <a name="pipeline-draft"></a>Ardışık düzen taslağı

Tasarımcıda bir işlem hattını düzenlerken, ilerleme durumu bir **ardışık düzen taslağı**olarak kaydedilir. Bir işlem hattı taslağını, modüller ekleyerek veya kaldırarak, işlem hedeflerini yapılandırarak, parametreler oluşturarak ve benzeri bir noktada düzenleyebilirsiniz.

Geçerli bir işlem hattı şu özelliklere sahiptir:

* Veri kümeleri yalnızca modüllere bağlanabilir.
* Modüller yalnızca veri kümelerine ya da diğer modüllere bağlanabilir.
* Modüller için tüm giriş bağlantı noktalarında veri akışına bir bağlantı bulunmalıdır.
* Her modül için gerekli tüm parametrelerin ayarlanması gerekir.

İşlem hattı taslağı çalıştırmaya hazırsanız bir işlem hattı çalıştırması gönderilir.

### <a name="pipeline-run"></a>İşlem hattı çalıştırması

İşlem hattını her çalıştırdığınızda işlem hattının yapılandırması ve sonuçları, çalışma alanınızda işlem **hattı çalıştırması**olarak depolanır. Sorun giderme veya denetim amaçlarıyla incelemek için herhangi bir işlem hattı çalıştırmasına geri dönebilirsiniz. Düzenlemeniz için yeni bir işlem hattı taslağı oluşturmak üzere bir işlem hattı çalıştırmasını **klonlayın** .

İşlem hattı çalıştırmaları, çalıştırma geçmişini düzenlemek için [denemeleri](concept-azure-machine-learning-architecture.md#experiments) olarak gruplandırılır. Her işlem hattı çalıştırması için denemeyi ayarlayabilirsiniz. 

## <a name="datasets"></a>Veri kümeleri

Machine Learning veri kümesi, verilerinize erişmeyi ve bunlarla çalışmayı kolaylaştırır. Bir dizi örnek veri kümesi, tasarımcı ile denemeler yapmak için eklenmiştir. Gerektiğinde daha [fazla veri kümesi kaydedebilirsiniz.](./how-to-create-register-datasets.md)

## <a name="module"></a>Modül

Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Tasarımcı, veri giriş işlevlerinden eğitim, Puanlama ve doğrulama işlemlerine kadar birçok modül içerir.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki Özellikler bölmesinde görüntülenir. Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz. Tasarımcıda bireysel modüller için işlem kaynaklarını ayarlayabilirsiniz. 

![Modül özellikleri](media/concept-designer/properties.png)

Kullanılabilir makine öğrenimi algoritmalarının kitaplığı ile ilgili bazı yardım için bkz. [algoritma & modül başvurusuna genel bakış](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>İşlem kaynakları

İş hattınızı çalıştırmak ve dağıtılan modellerinizi gerçek zamanlı uç noktalar veya ardışık düzen uç noktaları olarak barındırmak (Batch çıkarımı için) için çalışma alanınızdan işlem kaynaklarını kullanın. Desteklenen işlem hedefleri şunlardır:

| İşlem hedefi | Eğitim | Kurulum |
| ---- |:----:|:----:|
| Azure Machine Learning işlem | ✓ | |
| Azure Kubernetes Service | | ✓ |

İşlem hedefleri [Azure Machine Learning çalışma alanınıza](concept-workspace.md)iliştirilir. İşlem hedeflerinizi [Azure Machine Learning Studio (klasik)](https://ml.azure.com)içinde çalışma alanınızda yönetirsiniz.

## <a name="deploy"></a>Kurulum

Gerçek zamanlı bir işlem hattı gerçekleştirmek için, bir işlem hattını **gerçek zamanlı uç nokta**olarak dağıtmanız gerekir. Gerçek zamanlı uç nokta, bir dış uygulama ve Puanlama modeliniz arasında bir arabirim oluşturur. Gerçek zamanlı bir uç noktaya yapılan çağrı, uygulamaya gerçek zamanlı olarak tahmin sonuçları döndürür. Gerçek zamanlı bir uç noktaya çağrı yapmak için, uç noktasını dağıtırken oluşturulan API anahtarını geçirirsiniz. Uç nokta, Web programlama projeleri için popüler bir mimari seçimi olan REST 'i temel alır.

Gerçek zamanlı uç noktaların bir Azure Kubernetes hizmet kümesine dağıtılması gerekir.

Modelinizi dağıtmayı öğrenmek için bkz. [öğretici: tasarımcı ile makine öğrenimi modeli dağıtma](tutorial-designer-automobile-price-deploy.md).

## <a name="publish"></a>Yayımlama

Ardışık Düzen **uç noktasına**bir işlem hattı da yayımlayabilirsiniz. Gerçek zamanlı bir uç noktaya benzer şekilde, bir ardışık düzen uç noktası, REST çağrılarını kullanarak dış uygulamalardan yeni işlem hattı çalıştırmaları göndermenize olanak tanır. Ancak, bir ardışık düzen uç noktası kullanarak gerçek zamanlı olarak veri gönderemez veya alamazsınız.

Yayınlanan işlem hatları esnektir, modelleri eğitme veya yeniden eğitme, [toplu iş](how-to-run-batch-predictions-designer.md)için bilgi işlem gerçekleştirme, yeni verileri işleme ve çok daha fazlasını yapmak için kullanılabilirler. Çoklu işlem hatlarını tek bir ardışık düzen uç noktasına yayımlayabilir ve hangi ardışık düzen sürümünün çalıştırılacağını belirtebilirsiniz.

Yayımlanan bir işlem hattı, her modülün ardışık düzen taslağı içinde tanımladığınız işlem kaynakları üzerinde çalışır.

Tasarımcı, SDK ile aynı [Publishedpipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) nesnesini oluşturur.


## <a name="moving-from-the-visual-interface-to-the-designer"></a>Görsel arabirimden tasarımcıya geçme

Görsel Arabirim (Önizleme) güncelleştirildi ve artık Azure Machine Learning tasarımcı (Önizleme). Tasarımcı, Azure Machine Learning diğer özellikleriyle tamamen tümleştirilen bir ardışık düzen tabanlı arka uç kullanmak üzere tasarlanmıştır. 

Bu güncelleştirmelerin sonucunda, görsel arabirim için bazı kavramlar ve terimler değiştirilmiştir veya yeniden adlandırılacaktır. En önemli kavramsal değişiklikler için aşağıdaki tabloya bakın. 

| Tasarımcıda kavram | Daha önce görsel arabirimde |
| ---- |:----:|
| Ardışık düzen taslağı | Deneme |
| Gerçek zamanlı uç nokta | Web hizmeti |

### <a name="migrating-to-the-designer"></a>Tasarımcıya geçme

Varolan Visual Interface denemeleri ve Web hizmetlerini, tasarımcıda işlem hatlarına ve gerçek zamanlı uç noktalara dönüştürebilirsiniz. Visual Interface varlıklarınızı geçirmek için aşağıdaki adımları kullanın:

[!INCLUDE [migrate from the visual interface](../../../includes/aml-vi-designer-migration.md)]


## <a name="next-steps"></a>Sonraki adımlar

* Öğretici ile tahmine dayalı analiz ve makine öğrenimi hakkında temel bilgileri öğrenin [: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md) etme
* Örneklerden birini kullanın ve gereksinimlerinize göre değiştirin:

- [Örnek 1-gerileme: bir otomobil fiyatını tahmin edin](how-to-designer-sample-regression-automobile-price-basic.md)
- [Örnek 2-gerileme: otomobil fiyatlandırma için algoritmaları karşılaştırın](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Örnek 3-Özellik seçimi ile sınıflandırma: gelir tahmini](how-to-designer-sample-classification-predict-income.md)
- [Örnek 4-sınıflandırma: kredi riskini tahmin etme (maliyet duyarlı)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: karmaşıklığı tahmin etme](how-to-designer-sample-classification-churn.md)
- [Örnek 6-sınıflandırma: uçuş gecikmelerini tahmin etme](how-to-designer-sample-classification-flight-delay.md)
- [Örnek 7-metin sınıflandırması: Vikipedi SP 500 veri kümesi](how-to-designer-sample-text-classification.md)

