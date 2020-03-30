---
title: Azure Machine Learning'de oturum açmayı etkinleştirme
description: Varsayılan Python günlük paketini ve SDK'ya özgü işlevleri kullanarak Azure Machine Learning'de oturum açmayı nasıl etkinleştirebilirsiniz öğrenin.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 03/05/2020
ms.openlocfilehash: 73b9ae6bc3c15526bfdafd74330c7b86286631b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78396147"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Azure Machine Learning'de oturum açmayı etkinleştirme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning Python SDK, hem varsayılan Python günlük paketini kullanarak hem de hem yerel günlüğe kaydetme hem de portalda çalışma alanınıza günlüğe kaydetme için SDK'ya özgü işlevleri kullanarak günlük kaydetmeyi etkinleştirmenize olanak tanır. Günlükler geliştiricilere uygulama durumu hakkında gerçek zamanlı bilgi sağlar ve hataları veya uyarıları tanılamada yardımcı olabilir. Bu makalede, aşağıdaki alanlarda günlüğe kaydetmeyi etkinleştirmenin farklı yollarını öğreniyorsunuz:

> [!div class="checklist"]
> * Eğitim modelleri ve hesaplama hedefleri
> * Görüntü oluşturma
> * Dağıtılan modeller
> * Python `logging` ayarları

[Azure Machine Learning çalışma alanı oluşturun.](how-to-manage-workspace.md) Daha fazla bilgi için [Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) Kullanın SDK.

## <a name="training-models-and-compute-target-logging"></a>Eğitim modelleri ve hesaplama hedef günlüğü

Model eğitim işlemi sırasında günlüğe kaydetmeyi etkinleştirmenin birden çok yolu vardır ve gösterilen örnekler ortak tasarım modellerini gösterir. `Experiment` Sınıftaki `start_logging` işlevi kullanarak çalışmayla ilgili verileri bulutta çalışma alanınıza kolayca günlüğe kaydedebilirsiniz.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Ek günlük işlevleri için [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) sınıfının başvuru belgelerine bakın.

Eğitim ilerlemesi sırasında uygulama durumunun yerel `show_output` günlüğe kaydedilmesini etkinleştirmek için parametreyi kullanın. Ayrıntılı günlüğe kaydetmeyi etkinleştirmek, eğitim sürecindeki ayrıntıların yanı sıra uzak kaynaklar veya bilgi işlem hedefleri hakkındaki bilgileri görmenizi sağlar. Deneme gönderimi üzerine günlüğe kaydetmeyi etkinleştirmek için aşağıdaki kodu kullanın.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Ayrıca, ortaya çıkan çalıştırmadaki `wait_for_completion` işlevde aynı parametreyi de kullanabilirsiniz.

```python
run.wait_for_completion(show_output=True)
```

SDK ayrıca, eğitim için belirli senaryolarda varsayılan python günlüğe kaydetme paketinin kullanılmasını da destekler. Aşağıdaki örnek, bir `INFO` `AutoMLConfig` nesnenin günlüğe kaydetme düzeyini sağlar.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

Kalıcı bir işlem `show_output` hedefi oluştururken parametreyi de kullanabilirsiniz. Bilgi işlem hedef `wait_for_completion` oluşturma sırasında günlüğe kaydetmeyi etkinleştirmek için işlevdeki parametreyi belirtin.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Dağıtılan modeller için günlüğe kaydetme

Daha önce dağıtılan bir web hizmetinden günlükleri almak için `get_logs()` hizmeti yükleyin ve işlevi kullanın. Günlükler, dağıtım sırasında oluşan hatalar hakkında ayrıntılı bilgiler içerebilir.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Ayrıca, istek/yanıt sürelerini, hata oranlarını ve özel durumları izlemenize olanak tanıyan Uygulama Öngörüleri'ni etkinleştirerek web hizmetiniz için özel yığın izlerini günlüğe kaydedebilirsiniz. Uygulama `update()` Öngörüleri'ni etkinleştirmek için varolan bir web hizmetindeki işlevi arayın.

```python
service.update(enable_app_insights=True)
```

Daha fazla bilgi için, Monitör'e bakın [ve ML web hizmeti uç noktalarından veri toplayın.](how-to-enable-app-insights.md)

## <a name="python-native-logging-settings"></a>Python yerel günlük ayarları

SDK'daki belirli günlükler, günlük düzeyini HATA Ayıklama olarak ayarlamanızı sağlayan bir hata içerebilir. Günlüğe kaydetme düzeyini ayarlamak için komut dosyanıza aşağıdaki kodu ekleyin.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Sonraki adımlar

* [ML web hizmeti uç noktalarından veri izleme ve toplama](how-to-enable-app-insights.md)