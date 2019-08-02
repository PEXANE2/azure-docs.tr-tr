---
title: Azure Container Instances modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Container Instances kullanarak Azure Machine Learning hizmet modellerinizi Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2019
ms.openlocfilehash: 1bf8c22c2c7c8e8aaab35a4663652300a341c99c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571055"
---
# <a name="deploy-a-model-to-azure-container-instances"></a>Azure Container Instances model dağıtma

Azure Container Instances (ACI) üzerinde bir modeli Web hizmeti olarak dağıtmak için Azure Machine Learning hizmetini nasıl kullanacağınızı öğrenin. Aşağıdaki koşullardan biri doğru ise Azure Container Instances kullanın:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir. Zaman içinde acı kapsayıcıları oluşturmanız gerekmez. Dağıtım sürecinin bir parçası olarak oluşturulur.
- Geliştirilmekte olan bir modeli test edersiniz. 

ACI 'nin kota ve bölge kullanılabilirliği hakkında daha fazla bilgi için bkz. [Azure Container Instances Için kotalar ve bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) .

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](setup-create-workspace.md).

- Bir Machine Learning modeli, çalışma alanınıza kaydedildi. Kayıtlı bir modeliniz yoksa, bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk)veya [Azure Machine Learning Visual Studio Code uzantısı](how-to-vscode-tools.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlandığı varsayılır:

    * `ws`-Çalışma alanınıza ayarlayın.
    * `model`-Kayıtlı modelinize ayarlanır.
    * `inference_config`-Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

- Bu makaledeki __CLI__ kod parçacıkları bir `inferenceconfig.json` belge oluşturduğunuzu varsayar. Bu belgeyi oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

## <a name="deploy-to-aci"></a>ACI'ye dağıtma

Azure Container Instances bir modeli dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve Web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__ihtiyacınız vardır. Çıkarım yapılandırmasını oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AciWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-)
* [Model. deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)
* [WebService. wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. Kayıt `mymodel:1` , kayıtlı modelin adı ve sürümü ile değiştirin. Bu `myservice` hizmete verilecek adla değiştirin:

```azurecli-interactive
az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aci-deploy-config.md)]

Daha fazla bilgi için, [az ml model dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) başvurusuna bakın. 

## <a name="using-vs-code"></a>VS Code kullanma

Bkz. [vs Code modellerinizi dağıtma](how-to-vscode-tools.md#deploy-and-manage-models).

> [!IMPORTANT]
> Önceden test etmek için bir ACI kapsayıcısı oluşturmanız gerekmez. ACI kapsayıcıları gerektiğinde oluşturulur.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
