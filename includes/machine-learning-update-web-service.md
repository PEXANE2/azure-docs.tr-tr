---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: d36bf2db05113656a77e76ff900d95910f313c73
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79477264"
---
Bir Web hizmetini güncelleştirmek için `update` yöntemini kullanın. Web hizmetini yeni bir model, yeni bir giriş betiği veya bir çıkarım yapılandırmasında belirtime yeni bağımlılıklar kullanacak şekilde güncelleştirebilirsiniz. Daha fazla bilgi için, bkz. [WebService. Update](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#update--args-)belgeleri.

> [!IMPORTANT]
> Bir modelin yeni bir sürümünü oluşturduğunuzda, kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir.
>
> Azure Machine Learning tasarımcısından yayınlanan bir Web hizmetini güncelleştirmek için SDK 'Yı kullanamazsınız.

**SDK’yı kullanarak**

Aşağıdaki kod, bir Web hizmeti için model, ortam ve giriş betiğini güncelleştirmek üzere SDK 'nın nasıl kullanılacağını gösterir:

```python
from azureml.core import Environment
from azureml.core.webservice import Webservice
from azureml.core.model import Model, InferenceConfig

# Register new model.
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

# Use version 3 of the environment.
deploy_env = Environment.get(workspace=ws,name="myenv",version="3")
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=deploy_env)

service_name = 'myservice'
# Retrieve existing service.
service = Webservice(name=service_name, workspace=ws)



# Update to new model(s).
service.update(models=[new_model], inference_config=inference_config)
print(service.state)
print(service.get_logs())
```

**CLı 'yi kullanma**

Ayrıca, ML CLı kullanarak bir Web hizmetini güncelleştirebilirsiniz. Aşağıdaki örnek, yeni bir modeli kaydetmeyi ve sonra bir Web hizmetini yeni modeli kullanacak şekilde güncelleştirmeyi gösterir:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> Bu örnekte, kayıt komutundan model bilgilerini Update komutuna iletmek için bir JSON belgesi kullanılır.
>
> Hizmeti yeni bir giriş betiği veya ortamı kullanacak şekilde güncelleştirmek için bir [çıkarım yapılandırma dosyası](/azure/machine-learning/service/reference-azure-machine-learning-cli#inference-configuration-schema) oluşturun ve `ic` parametresini parametresiyle birlikte belirtin.

Daha fazla bilgi için, [az ml Service Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update) belgelerine bakın.