---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: 5102e8f75da14c58e948e81aaa418539dd18869a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159429"
---
`inferenceconfig.json` [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfının parametrelerine belge haritasındaki girişler. Aşağıdaki tabloda JSON belgesindeki varlıklar ile yöntemparametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Görüntü için çalışacak kodu içeren yerel bir dosyaya giden yol. |
| `sourceDirectory` | `source_directory` | İsteğe bağlı. Görüntüyü oluşturmak için tüm dosyaları içeren klasörlere giden yol, bu klasör veya alt klasördeki dosyalara erişmemi kolaylaştırır. Web hizmetine bağımlılık olarak yerel makinenizden bir klasörün tamamını yükleyebilirsiniz. Not: entry_script, conda_file ve extra_docker_file_steps yollarınız source_directory yoluna giden göreceli yollardır. |
| `environment` | `environment` | İsteğe bağlı.  Azure Machine Learning [ortamı.](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)|

Çıkarım yapılandırma dosyasına bir Azure Machine Learning [ortamının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) tam özelliklerini ekleyebilirsiniz. Bu ortam çalışma alanınızda yoksa, Azure Machine Learning bu ortamı oluşturur. Aksi takdirde, Azure Machine Learning gerekirse ortamı güncelleştirecektir. Aşağıdaki JSON bir örnektir:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "enabled": false,
            "sharedVolumes": true,
            "shmSize": null
        },
        "environmentVariables": {
            "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
        },
        "name": "my-deploy-env",
        "python": {
            "baseCondaEnvironment": null,
            "condaDependencies": {
                "channels": [
                    "conda-forge"
                ],
                "dependencies": [
                    "python=3.6.2",
                    {
                        "pip": [
                            "azureml-defaults",
                            "azureml-telemetry",
                            "scikit-learn",
                            "inference-schema[numpy-support]"
                        ]
                    }
                ],
                "name": "project_environment"
            },
            "condaDependenciesFile": null,
            "interpreterPath": "python",
            "userManagedDependencies": false
        },
        "version": "1"
    }
}
```

Ayrıca, ayrılmış CLI parametrelerinde varolan bir Azure Machine Learning [ortamını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) kullanabilir ve çıkarım yapılandırma dosyasından "ortam" anahtarını kaldırabilirsiniz. Ortam adı için -e'yi ve ortam sürümü için -ev'i kullanın. --ev' i belirtmezseniz, en son sürüm kullanılır. Çıkarım yapılandırma dosyasının bir örneği aşağıda verilmiştir:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

Aşağıdaki komut, önceki çıkarım yapılandırma dosyasını kullanarak bir modelin nasıl dağıtılangerektiğini gösterir (myInferenceConfig.json olarak adlandırılır). 

Ayrıca, varolan bir Azure Machine Learning [ortamının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) (AzureML-Minimal olarak adlandırılır) en son sürümünü de kullanır.

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```
