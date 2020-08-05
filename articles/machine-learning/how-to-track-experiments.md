---
title: ML denemeleri & ölçümlerini günlüğe kaydet
titleSuffix: Azure Machine Learning
description: Azure ML denemeleri izleyin ve model oluşturma işlemini iyileştirmek için çalışma ölçümlerini izleyin. Çalıştır. log, Run. start_logging veya ScriptRunConfig kullanarak eğitim betiğe günlük kaydı ekleyin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: d0c6488f9a75bbf9ba6775138edeed9c4a397abf
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552229"
---
# <a name="enable-logging-in-azure-ml-training-runs"></a>Azure ML eğitim çalıştırmaları 'nda günlüğü etkinleştirme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning Python SDK 'Sı, hem varsayılan Python günlük paketi hem de SDK 'ya özgü işlevselliği kullanarak gerçek zamanlı bilgileri günlüğe kaydetmenizi sağlar. Yerel olarak oturum açabilir ve portaldaki çalışma alanınıza Günlükler gönderebilirsiniz.

Günlükler, hataları ve uyarıları tanılamanıza veya parametreler ve model performansı gibi performans ölçümlerini izlemenize yardımcı olabilir. Bu makalede, günlüğü aşağıdaki senaryolarda nasıl etkinleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Etkileşimli eğitim oturumları
> * ScriptRunConfig kullanarak eğitim işleri gönderme
> * Python yerel `logging` ayarları
> * Ek kaynaklardan günlüğe kaydetme


> [!TIP]
> Bu makalede model eğitimi sürecinin nasıl izleneceği gösterilmektedir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning 'deki kaynak kullanımını ve olayları izlemeyi düşünüyorsanız, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="data-types"></a>Veri türleri

Skaler değerler, listeler, tablolar, resimler, dizinler ve daha fazlası dahil olmak üzere birden çok veri türünü günlüğe kaydedebilirsiniz. Daha fazla bilgi ve farklı veri türleri için Python kodu örnekleri için, [sınıf başvurusunu çalıştırma sayfasına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)bakın.

## <a name="interactive-logging-session"></a>Etkileşimli günlüğe kaydetme oturumu

Etkileşimli günlük oturumları genellikle Not defteri ortamlarında kullanılır. [Deneme. start_logging ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) yöntemi etkileşimli bir günlüğe kaydetme oturumu başlatır. Oturum sırasında günlüğe kaydedilen tüm ölçümler, denemenin içindeki çalıştırma kaydına eklenir. [Run. Completed ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) yöntemi oturumları sonlandırır ve çalıştırmayı tamamlandı olarak işaretler.

Aşağıdaki kod parçacığı, [Çalıştır. log ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#log-name--value--description----) yöntemiyle eğitim parametrelerini ve performans ölçümlerini günlüğe kaydetmek için etkileşimli bir oturum açma oturumu kullanır. Ayrıca, eğitilen modeli belirtilen bir çıkış konumuna yükler.

[! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb? Name = create_experiment)]

Etkileşimli günlüğe kaydetme kullanan tüm örnek bir not defteri için bkz. bir [Not defterinde model eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb).

## <a name="scriptrunconfig-logs"></a>ScriptRunConfig günlükleri

Bu bölümde, ScriptConfig çalıştırmaları içinde günlük kodu eklemeyi öğreneceksiniz. [**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) sınıfını, tekrarlanabilir çalıştırmalar için betikleri ve ortamları kapsüllemek için kullanabilirsiniz. Bu seçeneği, izleme için bir Visual jupi Not defteri pencere öğesini göstermek için de kullanabilirsiniz.

Bu örnek, alfa değerleri üzerinde bir parametre tarama işlemi gerçekleştirir ve [Run. log ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#log-name--value--description----) yöntemini kullanarak sonuçları yakalar.

1. Günlüğe kaydetme mantığını içeren bir eğitim betiği oluşturun `train.py` .

   [! Code-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. ```train.py```Betiği Kullanıcı tarafından yönetilen bir ortamda çalışacak şekilde gönderebilirsiniz. Tüm betik klasörü eğitim için gönderilir.

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = src)] [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? ad = Run)]

    `show_output`Parametresi, eğitim sürecinin ayrıntılarını ve uzak kaynaklar ya da işlem hedefleri hakkındaki bilgileri görmenizi sağlayan ayrıntılı günlük kaydını etkinleştirir. Denemeyi gönderdiğinizde ayrıntılı günlük kaydı açmak için aşağıdaki kodu kullanın.

```python
run = exp.submit(src, show_output=True)
```

Ayrıca, sonuç çalıştırmasında işlevinde aynı parametreyi de kullanabilirsiniz `wait_for_completion` .

```python
run.wait_for_completion(show_output=True)
```

ScriptRunConfigs günlüklerini kullanan tüm örnek bir not defteri için bkz. [modeli yerel olarak eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local/train-on-local.ipynb).

## <a name="native-python-logging"></a>Yerel Python günlüğü

SDK 'daki bazı Günlükler, günlüğe kaydetme düzeyini hata ayıklama olarak ayarlamanıza olanak tanıyan bir hata içerebilir. Günlüğe kaydetme düzeyini ayarlamak için betiğe aşağıdaki kodu ekleyin.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="additional-logging-sources"></a>Ek günlük kaynakları

Azure Machine Learning, otomatik makine öğrenimi çalıştırmaları veya işleri çalıştıran Docker kapsayıcıları gibi eğitim sırasında diğer kaynaklardaki bilgileri de günlüğe kaydedebilir. Bu Günlükler belgelenmemiştir, ancak sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişim kuradıysanız, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

Azure Machine Learning tasarımcısında (Önizleme) ölçümleri günlüğe kaydetme hakkında bilgi için bkz [. nasıl yapılır: tasarımcıda ölçümleri nasıl günlüğe kaydetme (Önizleme)](how-to-track-designer-experiments.md)

## <a name="example-notebooks"></a>Örnek not defterleri
Aşağıdaki Not defterleri bu makaledeki kavramları göstermektedir:
* [Nasıl yapılır kullanımı-bir not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [Nasıl yapılır-kullanım-azureml/eğitim/yerel olarak eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [Nasıl yapılır kullanımı-azureml/izleme-ve izleme-denemeleri/Logging-API](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanma hakkında daha fazla bilgi edinmek için şu makalelere bakın:

* [Azure Machine Learning tasarımcısında ölçümleri günlüğe kaydetme hakkında bilgi edinin (Önizleme)](how-to-track-designer-experiments.md).

* En iyi modeli nasıl kaydedeceğinizi ve öğreticide nasıl dağıtabileceğinizi gösteren bir örnek, [Azure Machine Learning ile bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)bölümüne bakın.
