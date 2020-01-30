---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: 6970732f53ffa99849e20f279c8bdf4160c30a0a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845161"
---
`inferenceconfig.json` belgesindeki girdiler, [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfının parametreleriyle eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Görüntüde çalıştırılacak kodu içeren yerel bir dosyanın yolu. |
| `runtime` | `runtime` | İsteğe bağlı. Görüntü için kullanılacak çalışma zamanı. Desteklenen çalışma zamanları `spark-py` ve `python`. `environment` ayarlandıysa, bu girdi yok sayılır. |
| `condaFile` | `conda_file` | İsteğe bağlı. Görüntü için kullanılacak Conda ortam tanımını içeren yerel bir dosyanın yolu.  `environment` ayarlandıysa, bu girdi yok sayılır. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | İsteğe bağlı. Görüntüyü ayarlarken çalıştırılacak ek Docker adımlarını içeren yerel bir dosyanın yolu.  `environment` ayarlandıysa, bu girdi yok sayılır.|
| `sourceDirectory` | `source_directory` | İsteğe bağlı. Görüntüyü oluşturmak için tüm dosyaları içeren klasörlerin yolu. Bu, bu klasör veya alt klasör içindeki tüm dosyalara erişmeyi kolaylaştırır. Yerel makinenizden bir klasörün tamamını Web hizmeti bağımlılıkları olarak yükleyebilirsiniz. Note: entry_script, conda_file ve extra_docker_file_steps yollarınız source_directory yolun göreli yollarıdır. |
| `enableGpu` | `enable_gpu` | İsteğe bağlı. Görüntüde GPU desteğinin etkinleştirilip etkinleştirilmeyeceğini belirtir. GPU görüntüsünün Azure Container Instances gibi bir Azure hizmetinde kullanılması gerekir. Örneğin, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti. Varsayılan değer false 'dur. `environment` ayarlandıysa, bu girdi yok sayılır.|
| `baseImage` | `base_image` | İsteğe bağlı. Temel görüntü olarak kullanılacak özel görüntü. Hiçbir temel görüntü sağlanmazsa, görüntü, belirtilen çalışma zamanı parametresini temel alır. `environment` ayarlandıysa, bu girdi yok sayılır. |
| `baseImageRegistry` | `base_image_registry` | İsteğe bağlı. Temel görüntüyü içeren görüntü kayıt defteri. `environment` ayarlandıysa, bu girdi yok sayılır.|
| `cudaVersion` | `cuda_version` | İsteğe bağlı. GPU desteği gerektiren görüntüler için yüklenecek CUDA sürümü. GPU görüntüsünün bir Azure hizmetinde kullanılması gerekir. Örneğin, Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti. Desteklenen sürümler 9,0, 9,1 ve 10,0. `enable_gpu` ayarlandıysa, varsayılan değer 9,1 ' dir. `environment` ayarlandıysa, bu girdi yok sayılır. |
| `description` | `description` | Görüntü için bir açıklama. `environment` ayarlandıysa, bu girdi yok sayılır.  |
| `environment` | `environment` | İsteğe bağlı.  Azure Machine Learning [ortamı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py).|

Aşağıdaki JSON, CLı ile kullanılacak örnek bir çıkarım yapılandırması örneğidir:

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```

Çıkarım yapılandırma dosyasına bir Azure Machine Learning [ortamının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) tam belirtimlerini dahil edebilirsiniz. Bu ortam çalışma alanınızda yoksa Azure Machine Learning oluşturacaktır. Aksi takdirde, Azure Machine Learning ortamı gerekirse güncelleştirir. Aşağıdaki JSON bir örnektir:

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

Ayrıca, ayrılmış CLı parametrelerinde mevcut bir Azure Machine Learning [ortamını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) kullanabilir ve "ortam" anahtarını çıkarım yapılandırma dosyasından kaldırabilirsiniz. Ortam adı için-e ve ortam sürümü için--KD kullanın. --KD belirtmezseniz, en son sürüm kullanılacaktır. Bir çıkarım yapılandırma dosyasına bir örnek aşağıda verilmiştir:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

Aşağıdaki komut, önceki çıkarım yapılandırma dosyasını (Myınenceconfig. JSON adlı) kullanarak bir modelin nasıl dağıtılacağını göstermektedir. 

Ayrıca, var olan bir Azure Machine Learning [ortamının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) en son sürümünü kullanır (yalnızca AzureML-minimal olarak adlandırılır).

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```
