---
title: Veri drizleme (Önizleme)
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetinin veri kayması için nasıl izleyebileceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: 13f73718fabd711e9c71a56ac4537b2ebef8a411
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371092"
---
# <a name="what-is-data-drift-monitoring-preview"></a>Veri kayması izleme (Önizleme) nedir?

Veri dağıtımı, verilerin dağıtımında değişiklik olduğunu. Makine öğrenimi bağlamında eğitilen makine öğrenimi modelleri, Drın nedeniyle düşürülmüş tahmin performansına neden olabilir. Öngörülere yönelik eğitim verileri ve tahmin yapmak için kullanılan veriler arasındaki izleme, performans sorunlarını algılamaya yardımcı olabilir.

Makine öğrenimi modelleri, yalnızca bunları eğmekte kullanılan veriler kadar iyidir. Modelinin performansını izlemeden izlemeye dağıtım, algılanmadan ve bu etkileri ortaya çıkmasına neden olabilir. Veri kayması izlemeyle, veri kayması 'nı algılayabilir ve bunlarla uyum sağlayabilirsiniz. 

## <a name="when-to-monitor-for-data-drift"></a>Veri kayması ne zaman izlenir?

İzleyebilmemiz için ölçümler şunları içerir:

+ Drın büyüklüğü (DRFT katsayısı)
+ Değişikliklerini 'in nedeni (özelliğe göre Drın katkısı)
+ Uzaklık ölçümleri (Wasserstein, enerji vb.)

Bu izleme söz konusu olduğunda, drfın algılandığında uyarı veya eylemler ayarlanabilir ve veri bilimcu sorunun asıl nedenini araştırabilir. 

Dağıtılan modelinize ait giriş verilerinin değişolabileceğini düşünüyorsanız, veri drmasını algılamayı kullanmayı göz önünde bulundurmanız gerekir.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde veri kayması nasıl izlenir

**Azure Machine Learning hizmetini**kullanarak veri kümeleri, veri kümeleri veya dağıtımlar aracılığıyla izlenir. Bir taban çizgisi veri kümesi (genellikle bir model için eğitim veri kümesi) için veri kayması izlemek üzere belirtilir. İkinci bir veri kümesi-genellikle bir dağıtımdan toplanan model giriş verileri, taban çizgisi veri kümesine göre test edilir. Her iki veri kümesi de veri Drın izleme hizmetine [profil](how-to-explore-prepare-data.md#explore-with-summary-statistics) oluşturulur ve giriş yapılır. Bir makine öğrenimi modeli, iki veri kümesi arasındaki farkları tespit etmek için eğitilir. Modelin performansı, iki veri kümesi arasındaki drifit 'in boyutunu ölçen DRFT katna dönüştürülür. [Model yorumlenebilirliğini](machine-learning-interpretability-explainability.md) kullanarak, değişikliklerini katlarıyla katkı sağlayan özellikler hesaplanır. Veri kümesi profilinden her bir özellik hakkındaki istatistiksel bilgiler izlenir. 

## <a name="data-drift-metric-output"></a>Veri drındaki ölçüm çıkışı

DRFT ölçümlerini görüntülemenin birden çok yolu vardır:

* [Jupyıter pencere öğesini kullanın.](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) `RunDetails`
* İşlevi herhangi bir `datadriftRun` nesne üzerinde kullanın. `get_metrics()`
* Modelinizdeki Azure portal ölçümleri görüntüleyin

Aşağıdaki ölçümler, bir veri Drın görevi için her bir çalıştırma yinelemesinde kaydedilir:

|Ölçüm|Açıklama|
--|--|
wasserstein_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklık.|
energy_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklık.|
datadrift_coefficient|Resmi olarak,-1 ile 1 arasında değişen gerçek sayı. Değişikliklerini bağlamında 0, değişikliklerini ve 1 ' in maksimum değişikliklerini olduğunu gösterir.|
datadrift_contribution|Drift 'e katkıda bulunan özelliklerin özellik önemi.|

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve veri kayması için nasıl izleneceğini öğrenin:

+ [Azure Kubernetes hizmeti (AKS) ile dağıtılan modellerdeki verileri izlemeyi öğrenin](how-to-monitor-data-drift.md)
+ [Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/) deneyin