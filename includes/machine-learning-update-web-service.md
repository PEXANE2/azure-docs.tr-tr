---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: d36bf2db05113656a77e76ff900d95910f313c73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477264"
---
Bir web hizmetini güncelleştirmek `update` için yöntemi kullanın. Yeni bir model, yeni bir giriş komut dosyası veya çıkarım yapılandırmasında belirtilebilen yeni bağımlılıkları kullanmak için web hizmetini güncelleştirebilirsiniz. Daha fazla bilgi için [Webservice.update](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#update--args-)belgelerine bakın.

> [!IMPORTANT]
> Bir modelin yeni bir sürümünü oluşturduğunuzda, kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir.
>
> Azure Machine Learning tasarımcısından yayınlanan bir web hizmetini güncelleştirmek için SDK'yı kullanamazsınız.

**SDK’yı kullanarak**

Aşağıdaki kod, bir web hizmetinin modelini, ortamını ve giriş komut dosyasını güncelleştirmek için SDK'nın nasıl kullanılacağını gösterir:

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

**CLI'yi kullanma**

Ayrıca ML CLI kullanarak bir web hizmetini güncelleyebilirsiniz. Aşağıdaki örnek, yeni bir model kaydetmeyi ve ardından yeni modeli kullanmak üzere bir web hizmetini güncelleştirmeyi gösterir:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> Bu örnekte, bir JSON belgesi, model bilgilerini kayıt komutundan güncelleştirme komutuna geçirmek için kullanılır.
>
> Yeni bir giriş komut dosyası veya ortamı kullanmak için hizmeti güncelleştirmek için, `ic` bir [çıkarım yapılandırma dosyası](/azure/machine-learning/service/reference-azure-machine-learning-cli#inference-configuration-schema) oluşturun ve parametre ile belirtin.

Daha fazla bilgi için [az ml hizmet güncelleştirme](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update) belgelerine bakın.