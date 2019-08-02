---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/26/2019
ms.author: larryfr
ms.openlocfilehash: fef6225812980900ad55944644310433a6105de2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68556861"
---
Yeni bir model oluşturduğunuzda, yeni modeli kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir. Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, bir Web hizmetinin modelini güncelleştirmek için SDK 'nın nasıl kullanılacağını gösterir:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name=service_name, workspace=ws)

# Update to new model(s)
service.update(models=[new_model])
print(service.state)
print(service.get_logs())
```

Ayrıca, ML CLı kullanarak bir Web hizmetini güncelleştirebilirsiniz. Aşağıdaki örnek, yeni bir modeli kaydetmeyi ve sonra Web hizmetini yeni modeli kullanacak şekilde güncelleştirmeyi gösterir:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> Bu örnekte, kayıt komutundan model bilgilerini Update komutuna iletmek için bir JSON belgesi kullanılır.