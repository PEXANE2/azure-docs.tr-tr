---
title: Dağıtılan bir Web hizmetini güncelleştirme
author: gvashishtha
ms.service: machine-learning
ms.topic: conceptual
ms.date: 07/31/2020
ms.author: gopalv
ms.openlocfilehash: a561a5fd865eba88f63690d39969961a87335def
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544651"
---
# <a name="update-a-deployed-web-service"></a>Dağıtılmış bir Web hizmetini güncelleştirme

Bu makalede, Azure Machine Learning ile dağıtılan bir Web hizmetinin dağıtılması gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, Azure Machine Learning ile zaten bir Web hizmeti dağıttığınız varsayılır. Bir Web hizmetini dağıtmayı öğrenmek isterseniz, [aşağıdaki adımları izleyin](how-to-deploy-and-where.md).

## <a name="update-web-service"></a>Web hizmetini Güncelleştir

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
> Hizmeti yeni bir giriş betiği veya ortamı kullanacak şekilde güncelleştirmek için bir [çıkarım yapılandırma dosyası](/azure/machine-learning/reference-azure-machine-learning-cli#inference-configuration-schema) oluşturun ve `ic` parametresini parametresiyle birlikte belirtin.

Daha fazla bilgi için, [az ml Service Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)
