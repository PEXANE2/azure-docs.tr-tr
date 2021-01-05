---
title: Web hizmetlerini güncelleştirme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ' de zaten dağıtılmış bir Web hizmetini yenilemeyi öğrenin. Model, ortam ve giriş betiği gibi ayarları güncelleştirebilirsiniz.
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: gopalv
author: gvashishtha
ms.date: 07/31/2020
ms.custom: deploy
ms.openlocfilehash: f955ee3f8bc150982c19107271fe7e7bdb1368cb
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97824569"
---
# <a name="update-a-deployed-web-service"></a>Dağıtılmış bir Web hizmetini güncelleştirme

Bu makalede, Azure Machine Learning ile dağıtılan bir Web hizmetini güncelleştirme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, Azure Machine Learning ile zaten bir Web hizmeti dağıttığınız varsayılır. Bir Web hizmetini dağıtmayı öğrenmek isterseniz, [aşağıdaki adımları izleyin](how-to-deploy-and-where.md).

## <a name="update-web-service"></a>Web hizmetini güncelleştirme

Bir Web hizmetini güncelleştirmek için `update` yöntemini kullanın. Web hizmetini yeni bir model, yeni bir giriş betiği veya bir çıkarım yapılandırmasında belirtime yeni bağımlılıklar kullanacak şekilde güncelleştirebilirsiniz. Daha fazla bilgi için, bkz. [WebService. Update](/python/api/azureml-core/azureml.core.webservice.webservice.webservice?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate--args-)belgeleri.

Bkz [. aks hizmeti güncelleştirme yöntemi.](/python/api/azureml-core/azureml.core.webservice.akswebservice?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate-image-none--autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--tags-none--properties-none--description-none--models-none--inference-config-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none--token-auth-enabled-none-)

Bkz [. acı hizmeti güncelleştirme yöntemi.](/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate-image-none--tags-none--properties-none--description-none--auth-enabled-none--ssl-enabled-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--enable-app-insights-none--models-none--inference-config-none-)

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
service.wait_for_deployment(show_output=True)
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
> Hizmeti yeni bir giriş betiği veya ortamı kullanacak şekilde güncelleştirmek için bir [çıkarım yapılandırma dosyası](./reference-azure-machine-learning-cli.md#inference-configuration-schema) oluşturun ve `ic` parametresini parametresiyle birlikte belirtin.

Daha fazla bilgi için, [az ml Service Update](/cli/azure/ext/azure-cli-ml/ml/service?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımın sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [TLS kullanarak Azure Machine Learning aracılığıyla web hizmetinin güvenliğini sağlama](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)
