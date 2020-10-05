---
title: ML denemelerini ve ölçümlerini günlüğe kaydetme
titleSuffix: Azure Machine Learning
description: Model oluşturma sürecini iyileştirmek için Azure ML denemelerinizi ve çalıştırma ölçümlerini izleyin. run.log, Run.start_logging veya ScriptRunConfig ile eğitim betiğinize günlüğe kaydetme özelliği ekleyin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 7eaa2fbe6033f801a252f6f2c7afa5eb726bce2f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318254"
---
# <a name="enable-logging-in-azure-ml-training-runs"></a>Azure ML eğitim çalıştırmalarında günlüğe kaydetmeyi etkinleştirme


Azure Machine Learning Python SDK'sı hem varsayılan Python günlüğe kaydetme paketini hem de SDK'ya özgü işlevleri kullanarak gerçek zamanlı günlük kaydı yapmanızı sağlar. Günlükleri yerel olarak kaydedebilir ve portaldaki çalışma alanınıza gönderebilirsiniz.

Günlükler; hataları ve uyarıları tanılamanıza veya parametreler ve model performansı gibi performans ölçümlerini takip etmenize yardımcı olabilir. Bu makalede aşağıdaki senaryolarda günlüğe kaydetmeyi nasıl etkinleştirebileceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Etkileşimli eğitim oturumları
> * ScriptRunConfig kullanarak eğitim işi gönderme
> * Python'daki yerel `logging` ayarları
> * Ek kaynaklardan günlüğe kaydetme


> [!TIP]
> Bu makalede model eğitim sürecini nasıl izleyebileceğiniz gösterilmiştir. Azure Machine Learning ile ilgili kaynak kullanımlarını ve olayları (kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanan model dağıtımları gibi) bkz. [Azure Machine Learning'i izleme](monitor-azure-machine-learning.md).

## <a name="data-types"></a>Veri türleri

Skaler değerler, listeler, tablolar, görüntüler ve dizinler gibi birçok farklı veri türünü günlüğe kaydedebilirsiniz. Farklı veri türleri hakkında daha fazla bilgi edinmek ve Python kodu örneklerini görmek için bkz. [Çalıştırma sınıfı başvuru sayfası](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true).

## <a name="interactive-logging-session"></a>Etkileşimli günlüğe kaydetme oturumu

Etkileşimli günlüğe kaydetme oturumları genellikle not defteri ortamlarında kullanılır. [Experiment.start_logging()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truestart-logging--args----kwargs-) yöntemi etkileşimli günlüğe kaydetme oturumu başlatır. Oturum sırasında günlüğe kaydedilen ölçümler, denemedeki çalıştırma kaydına eklenir. [run.complete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truecomplete--set-status-true-) yöntemi oturumları sonlandırır ve çalıştırmayı tamamlandı olarak işaretler.

## <a name="scriptrun-logs"></a>ScriptRun günlükleri

Bu bölümde ScriptConfig ile yapılandırıldığında oluşturulan çalıştırmalara günlüğe kaydetme kodu eklemeyi öğreneceksiniz. [**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) sınıfını kullanarak yinelenebilir çalıştırmalar için betikleri ve ortamları kapsülleyebilirsiniz. Bu seçeneği ayrıca izleme amacıyla görsel bir Jupyter Notebook pencere öğesi göstermek için de kullanabilirsiniz.

Bu örnek, [run.log()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truelog-name--value--description----) yöntemini kullanarak alfa değerleri üzerinde parametre tarama işlemi gerçekleştirir ve sonuçları yakalar.

1. Günlüğe kaydetme mantığını içeren bir eğitim betiği oluşturun: `train.py`.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. ```train.py``` betiğini kullanıcı tarafından yönetilen bir ortamda çalıştırılmak üzere gönderin. Betik klasörünün tamamı eğitim için gönderilir.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)] [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]

    `show_output` parametresi, eğitim sürecinin ayrıntılarına ek olarak uzak kaynaklar veya işlem hedefleri hakkındaki bilgileri görmenizi sağlayan ayrıntılı günlüğe kaydetme özelliğini etkinleştirir. Denemeyi gönderdiğinizde ayrıntılı günlüğe kaydetme özelliğini açmak için aşağıdaki kodu kullanın.

```python
run = exp.submit(src, show_output=True)
```

İsterseniz aynı parametreyi sonuç çalıştırmasındaki `wait_for_completion` işlevde de kullanabilirsiniz.

```python
run.wait_for_completion(show_output=True)
```

## <a name="native-python-logging"></a>Yerel Python günlüğü

SDK'daki bazı günlüklerde günlük kaydı düzeyini DEBUG olarak ayarlamanızı isteyen hatalar bulunabilir. Günlük kaydı düzeyini ayarlamak için aşağıdaki kodu betiğinize ekleyin.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="additional-logging-sources"></a>Ek günlüğe kaydetme kaynakları

Azure Machine Learning; eğitim sırasında otomatik makine öğrenmesi çalıştırmaları veya işleri çalıştıran Docker kapsayıcıları gibi farklı kaynaklardan alınan günlük bilgilerini de kullanabilir. Bu günlük kayıtları belge halinde sunulmaz ancak karşılaştığınız sorunlar için Microsoft destek ekibiyle iletişime geçerseniz sorun giderme aşamasında bu günlükler kullanılabilir.

Azure Machine Learning tasarımcısında ölçümleri günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Tasarımcıda ölçümleri günlüğe kaydetme](how-to-track-designer-experiments.md)

## <a name="example-notebooks"></a>Örnek not defterleri

Aşağıdaki not defterlerinde bu makaledeki kavramlar gösterilmiştir:
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning'i kullanma hakkında daha fazla bilgi edinmek için şu makaleleri inceleyin:

* [Azure Machine Learning tasarımcısında ölçümleri günlüğe kaydetmeyi](how-to-track-designer-experiments.md) öğrenin.

* En iyi modeli kaydetme ve dağıtma öğreticisi için bkz. [Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).
