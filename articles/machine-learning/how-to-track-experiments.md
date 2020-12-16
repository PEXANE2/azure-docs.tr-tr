---
title: ML denemelerini ve ölçümlerini günlüğe kaydetme
titleSuffix: Azure Machine Learning
description: Gerçek zamanlı çalıştırma ölçümlerini izlemek ve hataların ve uyarıların tanılanmasına yardımcı olmak için ML eğitim çalışmalarında günlüğe kaydetmeyi etkinleştirin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 0b91869e59109550910712b9386056c23843cecd
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559600"
---
# <a name="enable-logging-in-ml-training-runs"></a>ML eğitim çalıştırmaları 'nda günlüğü etkinleştir


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

Skaler değerler, listeler, tablolar, görüntüler ve dizinler gibi birçok farklı veri türünü günlüğe kaydedebilirsiniz. Farklı veri türleri hakkında daha fazla bilgi edinmek ve Python kodu örneklerini görmek için bkz. [Çalıştırma sınıfı başvuru sayfası](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py).

## <a name="interactive-logging-session"></a>Etkileşimli günlüğe kaydetme oturumu

Etkileşimli günlüğe kaydetme oturumları genellikle not defteri ortamlarında kullanılır. [Experiment.start_logging()](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truestart-logging--args----kwargs-) yöntemi etkileşimli günlüğe kaydetme oturumu başlatır. Oturum sırasında günlüğe kaydedilen ölçümler, denemedeki çalıştırma kaydına eklenir. [run.complete()](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truecomplete--set-status-true-) yöntemi oturumları sonlandırır ve çalıştırmayı tamamlandı olarak işaretler.

## <a name="scriptrun-logs"></a>ScriptRun günlükleri

Bu bölümde ScriptConfig ile yapılandırıldığında oluşturulan çalıştırmalara günlüğe kaydetme kodu eklemeyi öğreneceksiniz. [**ScriptRunConfig**](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) sınıfını kullanarak yinelenebilir çalıştırmalar için betikleri ve ortamları kapsülleyebilirsiniz. Bu seçeneği ayrıca izleme amacıyla görsel bir Jupyter Notebook pencere öğesi göstermek için de kullanabilirsiniz.

Bu örnek, [run.log()](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truelog-name--value--description----) yöntemini kullanarak alfa değerleri üzerinde parametre tarama işlemi gerçekleştirir ve sonuçları yakalar.

1. Günlüğe kaydetme mantığını içeren bir eğitim betiği oluşturun: `train.py`.

   [! kod-Python](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)


1. ```train.py``` betiğini kullanıcı tarafından yönetilen bir ortamda çalıştırılmak üzere gönderin. Betik klasörünün tamamı eğitim için gönderilir.

   [! Not defteri-Python](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src) 
    [! Not defteri-Python](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)

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

[! AML-kopya-Azure-Not defteri ekleme](/includes/aml-clone-for-examples.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning'i kullanma hakkında daha fazla bilgi edinmek için şu makaleleri inceleyin:

* [Azure Machine Learning tasarımcısında ölçümleri günlüğe kaydetmeyi](how-to-track-designer-experiments.md) öğrenin.

* En iyi modeli kaydetme ve dağıtma öğreticisi için bkz. [Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).
