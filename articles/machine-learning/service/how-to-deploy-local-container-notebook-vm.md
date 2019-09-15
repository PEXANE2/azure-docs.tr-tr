---
title: Modelleri dizüstü bilgisayar VM 'lerine dağıtma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modellerinizi Not defteri VM 'lerini kullanarak bir Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 08/08/2019
ms.openlocfilehash: 046f998038c47a48a8528bf36d87ac836395eec2
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002818"
---
# <a name="deploy-a-model-to-notebook-vms"></a>Dizüstü bilgisayar VM 'lerine model dağıtma

Azure Machine Learning kullanarak bir modeli Not defteri sanal makinenizde Web hizmeti olarak dağıtma hakkında bilgi edinin. Aşağıdaki koşullardan biri doğru ise Not defteri VM 'lerini kullanın:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir.
- Geliştirilmekte olan bir modeli test edersiniz.

> [!TIP]
> Bir not defteri VM 'deki Jupyter Notebook bir modeli aynı VM 'deki bir Web hizmetine dağıtmak _yerel bir dağıtımdır_. Bu durumda, ' yerel ' bilgisayar, Not defteri sanal bilgisayardır. Dağıtımlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Önkoşullar

- Bir not defteri VM 'si çalıştıran bir Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Kurulum ortamı ve çalışma alanı](tutorial-1st-experiment-sdk-setup.md).

## <a name="deploy-to-the-notebook-vms"></a>Not defteri VM 'lerine dağıtma

Not defteri sanal makinenize yerel dağıtımları gösteren örnek bir not defteri dahildir. Not defterini yüklemek ve modeli bir Web hizmeti olarak sanal makineye dağıtmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning Not defteri VM 'lerinizi seçin.

1. Alt dizini açın ve açın `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`. `samples-*` Açıldığında, Not defterini çalıştırın.

    ![Not defterindeki çalışan yerel hizmetin ekran görüntüsü](media/how-to-deploy-local-container-notebookvm/deploy-local-service.png)

1. Not defteri, hizmetin üzerinde çalıştığı URL 'YI ve bağlantı noktasını görüntüler. Örneğin: `https://localhost:6789`. Ayrıca, bağlantı noktasını göstermek için içeren `print('Local service port: {}'.format(local_service.port))` hücreyi de çalıştırabilirsiniz.

    ![Çalışan yerel hizmet bağlantı noktasının ekran görüntüsü](media/how-to-deploy-local-container-notebookvm/deploy-local-service-port.png)

1. Hizmeti not defteri VM 'sinden test etmek için `https://localhost:<local_service.port>` URL 'yi kullanın. Uzak bir istemciden test etmek için, Not defteri VM 'sinde çalışan hizmetin genel URL 'sini alın genel URL 'nin belirlenmesi için aşağıdaki formülü kullanın; `https://<notebookvm_name>-<local_service_port>.<azure_region_of_notebook>.notebooks.azureml.net/score`. Örneğin: `https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score`.

## <a name="test-the-service"></a>Hizmeti test etme

Çalışan hizmete örnek veri göndermek için aşağıdaki kodu kullanın. Değerini önceki adımdan URL `service_url` 'si ile değiştirin:

```python
import requests
import json
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')
access_token = "your bearer token"
headers = {'Content-Type':'application/json', 'Authorization': 'Bearer ' + access_token}
service_url = "https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score"
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