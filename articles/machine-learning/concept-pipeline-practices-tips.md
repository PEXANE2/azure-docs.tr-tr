---
title: Makine öğrenmesi işlem hatlarını yineleme ve geliştirme
titleSuffix: Azure Machine Learning
description: Hızlı geliştirme için desenler, uygulamalar ve ipuçları
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 05/01/2020
ms.openlocfilehash: 2ea353469ed111eebb591aa6ba86c652683cc2f0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82858187"
---
# <a name="iterating-and-evolving-machine-learning-pipelines"></a>Makine öğrenmesi işlem hatlarını yineleme ve geliştirme

Azure Machine Learning işlem hatları, kodunuzun modüle nleştirmek, sonuçları yeniden kullanmak ve işlem kaynaklarınızı iyileştirmek için etkili bir yol sağlar. İşlem hatları ile çalışmaya yönelik bazı pratik ipuçları ve uygulamalar aşağıda verilmiştir.

## <a name="how-do-you-get-started-with-pipelines"></a>İşlem hatlarına nasıl başladıysanız?

İşlem hatlarına yeni başladıysanız başlamak için birkaç seçenek vardır:

* Oluşturma işlemini okuyup yeniden oluşturarak en iyi şekilde karşılaşırsanız, [Azure MACHINE LEARNING SDK ile makine öğrenimi işlem hatlarını oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md) başlıklı makalede iyi bir uyum vardır 
* Jupyter not defterinde etkileşimli olarak öğrendiyseniz, Azure Machine Learning işlem hatlarında geliştirilen işlem hattı [:](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-getting-started.ipynb) Başlarken Not defteri sizin için uygun olabilir
* Önce bir kod durumu tercih ediyorsanız, [Azure Machine Learning Işlem hatları tanıtım](https://github.com/microsoft/aml-pipelines-demo) listesini kopyalamak iyi bir başlangıç noktası sağlar

## <a name="how-do-you-modularize-pipeline-code"></a>İşlem hattı kodunu nasıl moduirsiniz? 

Modüller ve `ModuleStep` sınıf, ml kodunuzu modüle nize etmek için harika bir fırsat sağlar. Ancak, işlem hattı adımları arasında hareket büyük ölçüde bir işlev çağrısından daha pahalı olduğunu göz önünde bulundurmanız gerekir. Sormanız gereken soru, bu işlevlerden ve verilerle kavramsal olarak bu diğer bölümden farklı değil mi? " Ancak "Bu işlevlerin ve verilerin ayrı olarak mi gelişmesi istiyorum?" or "Bu hesaplama pahalıdır ve çıktısını yeniden kullanabilir miyim?" Daha fazla bilgi için, bkz. The tebook [modül oluşturma, ModuleVersion ve ModuleStep ile bir işlem hattı içinde kullanma](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb).

Daha önce anlatıldığı gibi, eğitimin üzerinde veri hazırlığı yapmak genellikle bu fırsattan biridir. Bazen veri hazırlama karmaşık ve zaman alıcı, işlemi ayrı işlem hattı adımlarına bölebilmeniz yeterlidir. Diğer fırsatlar, eğitim sonrası testi ve analizi içerir. 

## <a name="how-do-you-speed-pipeline-iteration"></a>Ardışık düzen yinelemesini nasıl hızlıyorsunuz? 

Hızlı şekilde yineleme işlem hatlarını içeren yaygın teknikler şunlardır: 

- İşlem hattını kopyalama (kopya oluşturma) ve işlem hattını hızla yeniden çalıştırma
- Başlatma zamanından kaçınmak için işlem örneğinin çalışır durumda tutulması
- Verilerin yapılandırılması ve yeniden kullanmaya izin vermek için gereken adımlar, işlem hattının değişiklik verilerini kaldırma işlemini atlamasına izin verir

Hızlıca yinelemek istediğinizde, işlem hattınızı kopyalayabilir, işlem hattı oluşturabilir ve işlem hattını yeniden çalıştırabilirsiniz. Diğer bir faydalı teknik, Işlem normal durumda tutmanız durumunda yeni işlem gerçekleştirmenizin maliyeti olmayacaktır. Bir çalıştırmanın sonucunun yeniden kullanılmasına izin ver adımını ayarlarsanız, yinelenen yürütme mümkün olduğunda sonuçları yeniden kullanacaktır (adımlarda değişiklik olmadığında).

## <a name="how-do-you-collaborate-using-ml-pipelines"></a>ML işlem hatlarını kullanarak nasıl işbirliği yapabilirsiniz? 

Ayrı işlem hatları, efor 'in bölüneceği doğal hatlardır. Birden çok geliştirici veya hatta birden fazla ekip farklı adımlarda çalışarak, adımlar arasında akan veriler ve bağımsız değişkenler üzerine kabul edilir. 

Etkin geliştirme sırasında, `PipelineRun` `StepRun` çalışma alanından sonuçları alabilir ve çalıştırabilir, son ve ara çıktıyı indirmek için bu nesneleri kullanabilir ve bu yapıtları kendi modüle nleştirilmiş çalışmalarınız için kullanabilirsiniz.

## <a name="use-pipelines-to-test-techniques-in-isolation"></a>Teknikleri yalıtımda sınamak için işlem hatlarını kullanma

Gerçek dünyada ML çözümleri genellikle her adımın önemli ölçüde özelleştirilmesini içerir. Ham verilerin genellikle bir şekilde hazırlanması gerekir: filtrelenmiş, dönüştürülmüş ve genişletilmiş. Eğitim işlemleri birkaç olası mimaride bulunabilir ve ayrıntılı öğrenmede, katman boyutları ve etkinleştirme işlevlerine yönelik birçok olası varyasyon olabilir. Tutarlı bir mimaride bile hiper parametre arama, önemli bir şekilde WINS üretebilir.

[Otomatik ml](concept-automated-ml.md) ve [Otomatik hiper parametre arama](how-to-tune-hyperparameters.md)gibi araçlara ek olarak, Işlem hatları bir/B testi çözümü için önemli bir araç olabilir. Ardışık düzen adımlarınızın birkaç çeşidi varsa, türevlerini gerçekleştirmeye çalışan ayrı çalıştırmaları kolayca oluşturabilirsiniz: 

```python
data_preparation_variants = [data1, data2, data3]
data_augmentation_variants = [aug1, aug2]
architecture_variants = [train1, train2, train3]

# Cartesian product
all_variants = np.array(np.meshgrid(data_preparation_variants,data_augmentation_variants,architecture_variants)).T.reshape(-1,3)

pipelines = [Pipeline(workspace=ws, steps=variant.tolist(), description=str(variant)) for variant in all_variants]

```

