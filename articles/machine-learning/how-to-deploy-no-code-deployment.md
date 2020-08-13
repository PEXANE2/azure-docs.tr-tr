---
title: Kod dağıtımı yok (Önizleme)
titleSuffix: Azure Machine Learning
description: Giriş betiği olmadan bir model dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 07/31/2020
ms.topic: conceptual
ms.openlocfilehash: 9d6e234e1f4c8ac5199b92a09eb12bf7aa41b01b
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185494"
---
# <a name="preview-no-code-model-deployment"></a>Önizle Kod olmayan model dağıtımı

Şu anda önizleme aşamasında olan kod modeli dağıtımı yok ve şu makine öğrenimi çerçevelerini destekliyor:

## <a name="tensorflow-savedmodel-format"></a>TensorFlow SavedModel biçimi
TensorFlow modellerinin, kod olmayan model dağıtımıyla çalışmak için **savedmodel biçiminde** kaydedilmesi gerekir.

SavedModel oluşturma hakkında bilgi için [Bu bağlantıya](https://www.tensorflow.org/guide/saved_model) bakın.

[DockerHub 'A hizmet veren TensorFlow](https://registry.hub.docker.com/r/tensorflow/serving/tags)'Da "Etiketler" altında listelenen tüm TensorFlow sürümlerini destekliyoruz.

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='flowers',                        # Name of the registered model in your workspace.
                       model_path='./flowers_model',                # Local Tensorflow SavedModel folder to upload and register as a model.
                       model_framework=Model.Framework.TENSORFLOW,  # Framework used to create the model.
                       model_framework_version='1.14.0',            # Version of Tensorflow used to create the model.
                       description='Flowers model')

service_name = 'tensorflow-flower-service'
service = Model.deploy(ws, service_name, [model])
```

## <a name="onnx-models"></a>ONNX modelleri

ONNX model kaydı ve dağıtımı herhangi bir ONNX çıkarım grafiği için desteklenir. Ön işlem ve postprocess adımları şu anda desteklenmiyor.

Bir MNIST ONNX modelinin nasıl kaydedileceği ve dağıtılacağı hakkında bir örnek aşağıda verilmiştir:

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='mnist-sample',                  # Name of the registered model in your workspace.
                       model_path='mnist-model.onnx',              # Local ONNX model to upload and register as a model.
                       model_framework=Model.Framework.ONNX ,      # Framework used to create the model.
                       model_framework_version='1.3',              # Version of ONNX used to create the model.
                       description='Onnx MNIST model')

service_name = 'onnx-mnist-service'
service = Model.deploy(ws, service_name, [model])
```

Bir modeli öğrenmek için bkz. [Web hizmeti olarak dağıtılan Azure Machine Learning modelini](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)kullanma. Birçok ONNX projesi, hizmet tarafından beklenen veri biçiminin daha kolay olduğunu anlamak için, sıkı mağaza eğitimi ve doğrulama verilerine sahip prototip dosyalarını kullanır. Bir model geliştiricisi olarak, geliştiricileriniz için belgeleyebilirsiniz:

* Giriş biçimi (JSON veya ikili)
* Giriş veri şekli ve türü (örneğin, [100100, 3] şeklinin bir dizisi)
* Etki alanı bilgileri (örneğin, bir görüntü için, renk alanı, bileşen sırası ve değerlerin normalleştirilmiş olup olmadığı)

Pytorch kullanıyorsanız, bu [modelleri pytorch 'den ONNX 'e aktarmak](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) , dönüştürme ve sınırlamalara ilişkin ayrıntıları içerir. 

## <a name="scikit-learn-models"></a>Scikit-modelleri öğrenme

Tüm yerleşik scikit-öğrenme model türleri için hiçbir kod modeli dağıtımı desteklenmez.

Ek kod olmadan bir sköğren modelinin nasıl kaydedileceği ve dağıtıldığı hakkında bir örnek aşağıda verilmiştir:

```python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version='0.19.1',             # Version of scikit-learn used to create the model.
                       resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})
                       
service_name = 'my-sklearn-service'
service = Model.deploy(ws, service_name, [model])
```

> [!NOTE]
> Predict_proba destekleyen modeller, varsayılan olarak bu yöntemi kullanır. Bunu tahmin etmek için bunu geçersiz kılmak üzere, posta gövdesini aşağıda gösterildiği gibi değiştirebilirsiniz:

```python
import json


input_payload = json.dumps({
    'data': [
        [ 0.03807591,  0.05068012,  0.06169621, 0.02187235, -0.0442235,
         -0.03482076, -0.04340085, -0.00259226, 0.01990842, -0.01764613]
    ],
    'method': 'predict'  # If you have a classification model, the default behavior is to run 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

> [!NOTE]
> Bu bağımlılıklar önceden oluşturulmuş scikit-öğrenme çıkarımı kapsayıcısına dahil edilmiştir:

```yaml
    - dill
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
    - joblib
    - pandas
    - scipy
    - sklearn_pandas
```
## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)
