---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: a03f71adc99063fee4374b1436b08adf5bab783d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102511102"
---
Belgedeki girişler, `inferenceconfig.json` [ınenceconfig](/python/api/azureml-core/azureml.core.model.inferenceconfig) sınıfının parametreleriyle eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Description |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Görüntüde çalıştırılacak kodu içeren yerel bir dosyanın yolu. |
| `sourceDirectory` | `source_directory` | İsteğe bağlı. Görüntüyü oluşturmak için tüm dosyaları içeren klasörlerin yolu. Bu, bu klasör veya alt klasör içindeki tüm dosyalara erişmeyi kolaylaştırır. Yerel makinenizden bir klasörün tamamını Web hizmeti bağımlılıkları olarak yükleyebilirsiniz. Note: entry_script, conda_file ve extra_docker_file_steps yollarınız source_directory yolun göreli yollarıdır. |
| `environment` | `environment` | İsteğe bağlı.  Azure Machine Learning [ortamı](/python/api/azureml-core/azureml.core.environment.environment).|

Çıkarım yapılandırma dosyasına bir Azure Machine Learning [ortamının](/python/api/azureml-core/azureml.core.environment.environment) tam belirtimlerini dahil edebilirsiniz. Bu ortam çalışma alanınızda yoksa Azure Machine Learning oluşturacaktır. Aksi takdirde, Azure Machine Learning ortamı gerekirse güncelleştirir. Aşağıdaki JSON bir örnektir:

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
                            "scikit-learn==0.22.1",
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

Ayrıca, ayrılmış CLı parametrelerinde mevcut bir Azure Machine Learning [ortamını](/python/api/azureml-core/azureml.core.environment.environment) kullanabilir ve "ortam" anahtarını çıkarım yapılandırma dosyasından kaldırabilirsiniz. Ortam adı için-e ve ortam sürümü için--KD kullanın. --KD belirtmezseniz, en son sürüm kullanılacaktır. Bir çıkarım yapılandırma dosyasına bir örnek aşağıda verilmiştir:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

Aşağıdaki komut, önceki çıkarım yapılandırma dosyasını (myInferenceConfig.js) kullanarak bir modelin nasıl dağıtılacağını göstermektedir. 

Ayrıca, var olan bir Azure Machine Learning [ortamının](/python/api/azureml-core/azureml.core.environment.environment) en son sürümünü kullanır (yalnızca AzureML-minimal olarak adlandırılır).

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```