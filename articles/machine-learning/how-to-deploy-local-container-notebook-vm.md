---
title: Örnekleri hesaplamak için modelleri dağıtma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modellerinizi bilgi işlem örneklerini kullanarak web hizmeti olarak nasıl dağıtılayarak dağıtılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 03/05/2020
ms.openlocfilehash: 09164580b8bdb249fc12d14e827ad799d51cab34
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756583"
---
# <a name="deploy-a-model-to-azure-machine-learning-compute-instances"></a>Azure Machine Learning bilgi işlem örneklerine bir model dağıtma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning bilgi işlem örneğinde bir modeli web hizmeti olarak dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenin. Aşağıdaki koşullardan biri doğruysa, işlem örneklerini kullanın:

- Modelinizi hızla dağıtmanız ve doğrulamanız gerekir.
- Geliştirilmekte olan bir modeli test emlıyorsun.

> [!TIP]
> Bir jupyter Not Defteri'nden bir modeli bir işlem örneğinde, aynı VM'deki bir web hizmetine dağıtmak yerel bir _dağıtımdır._ Bu durumda, 'yerel' bilgisayar bilgi işlem örneğidir. Dağıtımlar hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

- Bir bilgi işlem örneği çalışan bir Azure Machine Learning çalışma alanı. Daha fazla bilgi için [Kurulum ortamı ve çalışma alanı'na](tutorial-1st-experiment-sdk-setup.md)bakın.

## <a name="deploy-to-the-compute-instances"></a>İşlem örneklerine dağıtma

Yerel dağıtımları gösteren örnek bir not defteri, işlem örneğinize dahildir. Not defterini yüklemek ve modeli VM'de bir web hizmeti olarak dağıtmak için aşağıdaki adımları kullanın:

1. [Azure Machine Learning stüdyosundan](https://ml.azure.com)Azure Machine Learning bilgi işlem örneklerinizi seçin.

1. Alt `samples-*` dizini açın ve `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`sonra açın. Açıldıktan sonra, not defterini çalıştırın.

    ![Not defterinde çalışan yerel hizmetin ekran görüntüsü](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service.png)

1. Not defteri, hizmetin üzerinde çalıştığının URL'sini ve bağlantı noktasını görüntüler. Örneğin, `https://localhost:6789`. Bağlantı noktasını görüntülemek için `print('Local service port: {}'.format(local_service.port))` içeren hücreyi de çalıştırabilirsiniz.

    ![Çalışan yerel hizmet bağlantı noktasının ekran görüntüsü](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service-port.png)

1. Hizmeti bir bilgi işlem örneğinden sınamak `https://localhost:<local_service.port>` için URL'yi kullanın. Uzak bir istemciden sınama yapmak için, bilgi işlem örneğinde çalışan hizmetin genel URL'sini alın. Genel URL aşağıdaki formülü kullanarak belirlenebilir; 
    * Dizüstü VM: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.notebooks.azureml.net/score`. 
    * İşlem örneği: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.instances.azureml.net/score`. 

    Örneğin, 
    * Dizüstü VM:`https://vm-name-6789.northcentralus.notebooks.azureml.net/score` 
    * İşlem örneği:`https://vm-name-6789.northcentralus.instances.azureml.net/score`

## <a name="test-the-service"></a>Hizmeti test etme

Örnek verileri çalışan hizmete göndermek için aşağıdaki kodu kullanın. Önceki adımdaki `service_url` URL ile değerini değiştirin:

> [!NOTE]
> Kimlik doğrulama, işlem örneğinde bir dağıtıma kimlik doğrulaması yaparken, kimlik doğrulaması Azure Etkin Dizini kullanılarak yapılır. Örnek `interactive_auth.get_authentication_header()` kodundaki arama, AAD'yi kullanarak kimliğinizi doğrular ve daha sonra bilgi işlem örneğindeki hizmete kimlik doğrulamak için kullanılabilecek bir üstbilgi döndürür. Daha fazla bilgi için bkz. Azure [Machine Learning kaynakları ve iş akışları için kimlik doğrulaması ayarlama.](how-to-setup-authentication.md#interactive-authentication)
>
> Azure Kubernetes Hizmeti veya Azure Kapsayıcı Örnekleri'nde dağıtımın kimliğini doğrularken farklı bir kimlik doğrulama yöntemi kullanılır. Hakkında daha fazla bilgi için bkz: [Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulaması ayarlama.](how-to-setup-authentication.md#web-service-authentication)

```python
import requests
import json
from azureml.core.authentication import InteractiveLoginAuthentication

# Get a token to authenticate to the compute instance from remote
interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()

# Create and submit a request using the auth header
headers = auth_header
# Add content type header
headers.update({'Content-Type':'application/json'})

# Sample data to send to the service
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

# Replace with the URL for your compute instance, as determined from the previous section
service_url = "https://vm-name-6789.northcentralus.notebooks.azureml.net/score"
# for a compute instance, the url would be https://vm-name-6789.northcentralus.instances.azureml.net/score
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Docker görüntüsünü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML Modelini tüketin](how-to-consume-web-service.md)
* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)