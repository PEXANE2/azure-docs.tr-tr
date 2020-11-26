---
title: Web hizmeti dağıtımının yerel olarak sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Ortak Azure Machine Learning Docker dağıtım hatalarını yerel olarak nasıl giderebileceğinizi, çözeceğinizi ve sorun gidermeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: gvashishtha
ms.author: gopalv
ms.reviewer: luquinta
ms.date: 11/25/2020
ms.topic: troubleshooting
ms.custom: devx-track-python, deploy, contperfq2
ms.openlocfilehash: 18de43cb170ccdede8f7fe13dc0c6c0c2fa4e7a3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188641"
---
# <a name="troubleshoot-model-deployment-locally"></a>Model dağıtımının yerel olarak sorunlarını giderme

Ortak Azure Machine Learning Docker Web hizmeti dağıtım hatalarını yerel olarak nasıl giderebileceğinizi ve çözeceğinizi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* Bir **Azure aboneliği**. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Yerel olarak hata ayıklamak için yerel sisteminizde çalışan bir Docker yüklemeniz olmalıdır.

    Docker yüklemenizi doğrulamak için, `docker run hello-world` bir Terminal veya komut isteminden komutunu kullanın. Docker 'ı yükleme veya Docker hataları sorunlarını giderme hakkında bilgi için bkz. [Docker belgeleri](https://docs.docker.com/).

## <a name="debug-locally"></a>Yerel olarak hata ayıkla

Bir modeli ACI veya AKS 'e dağıtırken sorun yaşıyorsanız, yerel bir Web hizmeti olarak dağıtın. Yerel web hizmetinin kullanılması sorunları gidermeyi kolaylaştırır.

Bir çalıştırılabilir örnek bulmak için [Machinelearningnotebook](https://github.com/Azure/MachineLearningNotebooks) deposunda örnek bir [Yerel dağıtım Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local.ipynb) bulabilirsiniz.

> [!WARNING]
> Yerel Web hizmeti dağıtımları, üretim senaryolarında desteklenmez.

Yerel olarak dağıtmak için kodunuzu `LocalWebservice.deploy_configuration()` bir dağıtım yapılandırması oluşturmak üzere değiştirin. Ardından `Model.deploy()` hizmeti dağıtmak için kullanın. Aşağıdaki örnek, bir modeli (model değişkeninde yer alan) yerel bir Web hizmeti olarak dağıtır:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice


# Create inference configuration based on the environment definition and the entry script
myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Kendi Conda Specification YAML 'nizi tanımlıyorsanız, 1.0.45 = bir PIP bağımlılığı olarak >=. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereklidir.

Bu noktada, hizmetle normal şekilde çalışabilirsiniz. Aşağıdaki kod, hizmete veri gönderilmesini göstermektedir:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

Python ortamınızı özelleştirme hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md). 

### <a name="update-the-service"></a>Hizmeti güncelleştirme

Yerel test sırasında, `score.py` kayıt eklemek veya bulduğunuz herhangi bir sorunu çözmeye çalışmak için dosyayı güncelleştirmeniz gerekebilir. Değişiklikleri dosyaya yeniden yüklemek için `score.py` kullanın `reload()` . Örneğin, aşağıdaki kod, hizmeti için betiği yeniden yükler ve ardından verileri bu sunucuya gönderir. Veriler, güncelleştirilmiş dosya kullanılarak puanlanır `score.py` :

> [!IMPORTANT]
> `reload`Yöntemi yalnızca yerel dağıtımlar için kullanılabilir. Bir dağıtımı başka bir işlem hedefine güncelleştirme hakkında daha fazla bilgi için bkz. [Web weblerinizi güncelleştirme](how-to-deploy-update-web-service.md).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Betik, `InferenceConfig` hizmet tarafından kullanılan nesne tarafından belirtilen konumdan yeniden yüklenir.

Modeli, Conda bağımlılıklarını veya dağıtım yapılandırmasını değiştirmek için [Update ()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate--args-)kullanın. Aşağıdaki örnek, hizmet tarafından kullanılan modeli güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti Sil

Hizmeti silmek için [Delete ()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedelete--)kullanın.

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Docker günlüğünü İnceleme

Service nesnesinden ayrıntılı Docker motoru günlük iletilerini yazdırabilirsiniz. ACI, AKS ve yerel dağıtımlar için günlüğü görüntüleyebilirsiniz. Aşağıdaki örnek günlükleri nasıl yazdırabileceğinizi gösterir.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

Bir satırı `Booting worker with pid: <pid>` günlüklerde birden çok kez görürseniz, çalışanı başlatmak için yeterli bellek yok demektir.
İçindeki değerini artırarak hatayı ele alabilirsiniz. `memory_gb``deployment_config`

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Uzak dağıtımlarla ilgili sorunları giderme](how-to-troubleshoot-deployment.md)
* [Dağıtım ve nerede](how-to-deploy-and-where.md)
* [Öğretici: eğitim & dağıtım modelleri](tutorial-train-models-with-aml.md)
* [Denemeleri yerel olarak nasıl çalıştırılır ve hata ayıklayın](./how-to-debug-visual-studio-code.md)
