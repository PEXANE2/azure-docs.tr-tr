---
title: Modelleri Azure Kapsayıcı Örneklerine dağıtma
titleSuffix: Azure Machine Learning
description: Azure Kapsayıcı Örnekleri'ni kullanarak Azure Machine Learning modellerinizi bir web hizmeti olarak nasıl dağıtılayarak dağıtılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 12/27/2019
ms.openlocfilehash: cb5603c12f06e4f5d5675ca27e171321ecc8827a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536454"
---
# <a name="deploy-a-model-to-azure-container-instances"></a>Azure Kapsayıcı Örneklerine bir model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Kapsayıcı Örnekleri'nde (ACI) bir modeli web hizmeti olarak dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenin. Aşağıdaki koşullardan biri doğruysa Azure Kapsayıcı Örnekleri'ni kullanın:

- Modelinizi hızla dağıtmanız ve doğrulamanız gerekir. ACI kapsayıcılarını önceden oluşturmanız gerekmez. Dağıtım işleminin bir parçası olarak oluşturulurlar.
- Geliştirilmekte olan bir modeli test emlıyorsun. 

ACI için kota ve bölge kullanılabilirliği hakkında bilgi için Azure Kapsayıcı Örnekleri makalesi [için Kotalar ve bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) makalesine bakın.

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

- Çalışma alanınızda kayıtlı bir makine öğrenme modeli. Kayıtlı bir modeliniz yoksa, [modelleri nasıl ve nerede dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

- [Machine Learning hizmeti için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), Azure Machine Learning Python [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlı olduğunu varsayar:

    * `ws`- Çalışma alanınıza ayarlayın.
    * `model`- Kayıtlı modelinize ayarlayın.
    * `inference_config`- Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)hakkında bilgi alabiliyorum.

- Bu makaledeki __CLI__ parçacıkları bir `inferenceconfig.json` belge oluşturduğunuzu varsayar. Bu belgeyi oluşturma hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)hakkında bilgi alabiliyorum.

## <a name="deploy-to-aci"></a>ACI'ye dağıtma

Bir modeli Azure Kapsayıcı Örneklerine dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__da ihtiyacınız vardır. Çıkarım yapılandırması oluşturma hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)konusunda bakın.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
from azureml.core.webservice import AciWebservice, Webservice
from azureml.core.model import Model

deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Bu örnekte kullanılan sınıflar, yöntemler ve parametreler hakkında daha fazla bilgi için aşağıdaki başvuru belgelerine bakın:

* [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)
* [Model.deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [Webservice.wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLI'yi kullanma

CLI kullanarak dağıtmak için aşağıdaki komutu kullanın. Kayıtlı `mymodel:1` modelin adı ve sürümüyle değiştirin. Bu `myservice` hizmeti vermek için adla değiştirin:

```azurecli-interactive
az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

Daha fazla bilgi için [az ml modeli dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) referansına bakın. 

## <a name="using-vs-code"></a>VS Code'u kullanma

[Vs Kodu ile modellerinizi dağıtın](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model)bakın.

> [!IMPORTANT]
> Önceden test etmek için bir ACI kapsayıcı oluşturmanız gerekmez. ACI kapsayıcıları gerektiği gibi oluşturulur.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirme

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Docker görüntüsünü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorun giderme](how-to-troubleshoot-deployment.md)
* [SSL ile Güvenli Azure Machine Learning web hizmetleri](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML Modelini tüketin](how-to-consume-web-service.md)
* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)
