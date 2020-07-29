---
title: Azure Machine Learning oturum açmayı etkinleştir
description: Hem varsayılan Python günlüğü paketini hem de SDK 'ya özgü işlevselliği kullanarak Azure Machine Learning oturum açmayı nasıl etkinleştireceğinizi öğrenin.
ms.author: larryfr
author: BlackMist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 03/05/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: d4fb08a618c6dfb35f3f8d154af6820c92d62781
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320145"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Azure Machine Learning oturum açmayı etkinleştir
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning Python SDK 'Sı, hem varsayılan Python günlüğü paketini hem de yerel günlüğe kaydetme ve portalda çalışma alanınıza oturum açmak için SDK 'ya özgü işlevselliği kullanarak günlüğü etkinleştirmenizi sağlar. Günlükler geliştiricilere uygulama durumu hakkında gerçek zamanlı bilgiler sağlar ve hataları veya uyarıları tanılamanıza yardımcı olabilir. Bu makalede, aşağıdaki alanlarda günlüğü etkinleştirmenin farklı yollarını öğreneceksiniz:

> [!div class="checklist"]
> * Eğitim modelleri ve işlem hedefleri
> * Görüntü oluşturma
> * Dağıtılan modeller
> * Python `logging` ayarları

[Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md). SDK hakkında daha fazla bilgi için [Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) kullanın.

## <a name="training-models-and-compute-target-logging"></a>Eğitim modelleri ve işlem hedefi günlüğü

Model eğitimi işlemi sırasında günlüğü etkinleştirmenin birden çok yolu vardır ve gösterilen örneklerde yaygın tasarım desenleri gösterilmektedir. Sınıftaki işlevini kullanarak, çalıştırmada ilgili verileri bulutta çalışma alanınıza kolayca kaydedebilirsiniz `start_logging` `Experiment` .

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Ek günlüğe kaydetme işlevlerine yönelik [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) sınıfı için başvuru belgelerine bakın.

Eğitim süreci sırasında uygulama durumunun yerel günlük kaydını etkinleştirmek için `show_output` parametresini kullanın. Ayrıntılı günlüğün etkinleştirilmesi, eğitim sürecinin ayrıntılarını ve uzak kaynaklar veya işlem hedefleri hakkındaki bilgileri görmenizi sağlar. Deneme gönderimi sırasında günlüğe kaydetmeyi etkinleştirmek için aşağıdaki kodu kullanın.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Ayrıca, sonuç çalıştırmasında işlevinde aynı parametreyi de kullanabilirsiniz `wait_for_completion` .

```python
run.wait_for_completion(show_output=True)
```

SDK Ayrıca, eğitim için belirli senaryolarda varsayılan Python günlük paketi ' ni kullanmayı da destekler. Aşağıdaki örnek, bir nesnesinde bir günlüğe kaydetme düzeyi sunar `INFO` `AutoMLConfig` .

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

`show_output`Kalıcı bir işlem hedefi oluştururken parametresini de kullanabilirsiniz. `wait_for_completion`İşlem hedefi oluşturma sırasında günlüğe kaydetmeyi etkinleştirmek için işlevinde parametresini belirtin.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Dağıtılan modeller için günlüğe kaydetme

Daha önce dağıtılan bir Web hizmetinden günlükleri almak için hizmeti yükleyin ve `get_logs()` işlevini kullanın. Günlükler, dağıtım sırasında oluşan hatalar hakkında ayrıntılı bilgiler içerebilir.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Ayrıca, istek/yanıt sürelerini, hata oranlarını ve özel durumları izlemenize olanak tanıyan Application Insights etkinleştirerek Web hizmetiniz için özel yığın izlemelerini günlüğe kaydedebilirsiniz. `update()`Application Insights sağlamak için var olan bir Web hizmetinde işlevi çağırın.

```python
service.update(enable_app_insights=True)
```

Daha fazla bilgi için bkz. [ml Web hizmeti uç noktalarından verileri izleme ve toplama](how-to-enable-app-insights.md).

## <a name="python-native-logging-settings"></a>Python yerel günlük kaydı ayarları

SDK 'daki belirli günlüklerde, günlüğe kaydetme düzeyini hata ayıklama olarak ayarlamanıza olanak tanıyan bir hata bulunabilir. Günlüğe kaydetme düzeyini ayarlamak için betiğe aşağıdaki kodu ekleyin.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Sonraki adımlar

* [ML Web hizmeti uç noktalarından verileri izleme ve toplama](how-to-enable-app-insights.md)
