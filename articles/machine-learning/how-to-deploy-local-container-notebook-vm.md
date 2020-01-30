---
title: İşlem örneklerine model dağıtma
titleSuffix: Azure Machine Learning
description: İşlem örnekleri kullanarak Azure Machine Learning modellerinizi Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 10/25/2019
ms.openlocfilehash: 45f9f9d1c9c8414f7d3c97aab05b012a16079f40
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843540"
---
# <a name="deploy-a-model-to-azure-machine-learning-compute-instances"></a>Azure Machine Learning işlem örneklerine model dağıtma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!NOTE]
> İşlem örnekleri (Önizleme) yalnızca **Orta Kuzey ABD**, * * Doğu ABD 2 * *, **Kuzey Avrupa** veya **UK Güney**olan çalışma alanları için kullanılabilir.
>Çalışma alanınız başka bir bölgedeyse, bunun yerine bir [Not DEFTERI VM](concept-compute-instance.md#notebookvm) 'si oluşturmaya ve kullanmaya devam edebilirsiniz.  Bu makaledeki adımları kullanarak bir modeli, bir işlem örneğine veya bir not defteri VM 'sine dağıtabilirsiniz.

Azure Machine Learning kullanarak bir modeli Azure Machine Learning işlem örneğiniz üzerinde Web hizmeti olarak dağıtma hakkında bilgi edinin. Aşağıdaki koşullardan biri doğru ise, işlem örnekleri kullanın:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir.
- Geliştirilmekte olan bir modeli test edersiniz.

> [!TIP]
> Bir işlem örneğindeki Jupyter Notebook bir modeli aynı VM 'deki bir Web hizmetine dağıtmak _yerel bir dağıtımdır_. Bu durumda, ' yerel ' bilgisayar işlem örneğidir. Dağıtımlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Ön koşullar

- İşlem örneği çalıştıran bir Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Kurulum ortamı ve çalışma alanı](tutorial-1st-experiment-sdk-setup.md).

## <a name="deploy-to-the-compute-instances"></a>İşlem örneklerine dağıtın

Yerel dağıtımları gösteren örnek bir not defteri, işlem örneğinize dahil edilmiştir. Not defterini yüklemek ve modeli bir Web hizmeti olarak sanal makineye dağıtmak için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com)'dan Azure Machine Learning işlem örneklerinizi seçin.

1. `samples-*` alt dizinini açın ve `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`açın. Açıldığında, Not defterini çalıştırın.

    ![Not defterindeki çalışan yerel hizmetin ekran görüntüsü](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service.png)

1. Not defteri, hizmetin üzerinde çalıştığı URL 'YI ve bağlantı noktasını görüntüler. Örneğin, `https://localhost:6789`. Bağlantı noktasını göstermek için `print('Local service port: {}'.format(local_service.port))` içeren hücreyi de çalıştırabilirsiniz.

    ![Çalışan yerel hizmet bağlantı noktasının ekran görüntüsü](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service-port.png)

1. Hizmeti bir işlem örneğinden test etmek için `https://localhost:<local_service.port>` URL 'sini kullanın. Uzak bir istemciden test etmek için, işlem örneğinde çalışan hizmetin genel URL 'sini alın. Ortak URL belirlenebilir ve şu formülü kullanın; 
    * Not defteri VM 'si: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.notebooks.azureml.net/score`. 
    * İşlem örneği: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.instances.azureml.net/score`. 

    Örneğin, 
    * Not defteri VM 'si: `https://vm-name-6789.northcentralus.notebooks.azureml.net/score` 
    * İşlem örneği: `https://vm-name-6789.northcentralus.instances.azureml.net/score`

## <a name="test-the-service"></a>Hizmeti test etme

Çalışan hizmete örnek veri göndermek için aşağıdaki kodu kullanın. `service_url` değerini önceki adımdaki URL ile değiştirin:

> [!NOTE]
> İşlem örneğindeki bir dağıtımda kimlik doğrulanırken, Azure Active Directory kullanılarak kimlik doğrulaması yapılır. Örnek kodda `interactive_auth.get_authentication_header()` çağrısı, AAD 'yi kullanarak kimlik doğrular ve daha sonra işlem örneğindeki hizmette kimlik doğrulaması yapmak için kullanılabilecek bir üst bilgi döndürür. Daha fazla bilgi için bkz. [Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarlama](how-to-setup-authentication.md#interactive-authentication).
>
> Azure Kubernetes hizmetinde veya Azure Container Instances bir dağıtımda kimlik doğrulanırken, farklı bir kimlik doğrulama yöntemi kullanılır. Hakkında daha fazla bilgi için bkz. [Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarlama](how-to-setup-authentication.md#web-service-authentication).

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

* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)