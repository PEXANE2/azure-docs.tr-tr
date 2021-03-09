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
ms.openlocfilehash: 3d970193bd8d73baeac89fb45da4c8a3d81cbde4
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102518408"
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

Skaler değerler, listeler, tablolar, görüntüler ve dizinler gibi birçok farklı veri türünü günlüğe kaydedebilirsiniz. Farklı veri türleri hakkında daha fazla bilgi edinmek ve Python kodu örneklerini görmek için bkz. [Çalıştırma sınıfı başvuru sayfası](/python/api/azureml-core/azureml.core.run%28class%29).

### <a name="logging-run-metrics"></a>Günlük çalıştırma ölçümleri 

Ölçüm görselleştirmelerini etkilemek için günlüğe kaydetme API 'Lerinde aşağıdaki yöntemleri kullanın. Günlüğe kaydedilen bu ölçümler için [hizmet sınırlarını](https://docs.microsoft.com/azure/machine-learning/resource-limits-quotas-capacity#metrics) göz önünde edin. 

|Günlüğe kaydedilen değer|Örnek kod| Portalda biçim|
|----|----|----|
|Sayısal değerlerden oluşan diziyi günlüğe kaydet| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Aynı ölçüm adı ile birlikte sürekli kullanılan tek bir sayısal değeri günlüğe kaydet (for döngüsü içinde olduğu gibi)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|2 sayısal sütundan oluşan bir satırı sürekli olarak günlüğe kaydet|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişkenli çizgi grafik|
|2 sayısal sütun içeren günlük tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişkenli çizgi grafik|
|Günlük resmi|`run.log_image(name='food', path='./breadpudding.jpg', plot=None, description='desert')`|Çalışma için bir görüntü dosyası veya Matplotlib çizimi günlüğe kaydetmek için bu yöntemi kullanın. Bu görüntüler, çalıştırma kaydında görünür ve karşılaştırılabilir olacaktır|

### <a name="logging-with-mlflow"></a>MLflow ile günlüğe kaydetme
Ölçümleri günlüğe kaydetmek için Mlflowgünlükçüsü kullanın.

```python
from azureml.core import Run
# connect to the workspace from within your running code
run = Run.get_context()
ws = run.experiment.workspace

# workspace has associated ml-flow-tracking-uri
mlflow_url = ws.get_mlflow_tracking_uri()

#Example: PyTorch Lightning
from pytorch_lightning.loggers import MLFlowLogger

mlf_logger = MLFlowLogger(experiment_name=run.experiment.name, tracking_uri=mlflow_url)
mlf_logger._run_id = run.id
```

## <a name="interactive-logging-session"></a>Etkileşimli günlüğe kaydetme oturumu

Etkileşimli günlüğe kaydetme oturumları genellikle not defteri ortamlarında kullanılır. [Experiment.start_logging()](/python/api/azureml-core/azureml.core.experiment%28class%29#start-logging--args----kwargs-) yöntemi etkileşimli günlüğe kaydetme oturumu başlatır. Oturum sırasında günlüğe kaydedilen ölçümler, denemedeki çalıştırma kaydına eklenir. [run.complete()](/python/api/azureml-core/azureml.core.run%28class%29#complete--set-status-true-) yöntemi oturumları sonlandırır ve çalıştırmayı tamamlandı olarak işaretler.

## <a name="scriptrun-logs"></a>ScriptRun günlükleri

Bu bölümde ScriptConfig ile yapılandırıldığında oluşturulan çalıştırmalara günlüğe kaydetme kodu eklemeyi öğreneceksiniz. [**ScriptRunConfig**](/python/api/azureml-core/azureml.core.scriptrunconfig) sınıfını kullanarak yinelenebilir çalıştırmalar için betikleri ve ortamları kapsülleyebilirsiniz. Bu seçeneği ayrıca izleme amacıyla görsel bir Jupyter Notebook pencere öğesi göstermek için de kullanabilirsiniz.

Bu örnek, [run.log()](/python/api/azureml-core/azureml.core.run%28class%29#log-name--value--description----) yöntemini kullanarak alfa değerleri üzerinde parametre tarama işlemi gerçekleştirir ve sonuçları yakalar.

1. Günlüğe kaydetme mantığını içeren bir eğitim betiği oluşturun: `train.py`.

   [!code-python[](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. ```train.py``` betiğini kullanıcı tarafından yönetilen bir ortamda çalıştırılmak üzere gönderin. Betik klasörünün tamamı eğitim için gönderilir.

   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = src)]


   [! Not defteri-Python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? ad = Run)]

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
