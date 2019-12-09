---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/06/2019
ms.author: larryfr
ms.openlocfilehash: 52689c585e148c18d16ad627570414a8de444fea
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926955"
---
`inferenceconfig.json` belgesindeki girdiler, [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfının parametreleriyle eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Görüntüde çalıştırılacak kodu içeren yerel bir dosyanın yolu. |
| `runtime` | `runtime` | İsteğe bağlı. Görüntü için kullanılacak çalışma zamanı. Desteklenen geçerli çalışma zamanları `spark-py` ve `python`. `environment` ayarlandıysa, bu yok sayılır. |
| `condaFile` | `conda_file` | İsteğe bağlı. Görüntü için kullanılacak Conda ortam tanımını içeren yerel bir dosyanın yolu.  `environment` ayarlandıysa, bu yok sayılır. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | İsteğe bağlı. Görüntüyü ayarlarken çalıştırılacak ek Docker adımlarını içeren yerel bir dosyanın yolu.  `environment` ayarlandıysa, bu yok sayılır.|
| `sourceDirectory` | `source_directory` | İsteğe bağlı. Görüntüyü oluşturmak için tüm dosyaları içeren klasörlerin yolu. |
| `enableGpu` | `enable_gpu` | İsteğe bağlı. Görüntüde GPU desteğinin etkinleştirilip etkinleştirilmeyeceğini belirtir. GPU görüntüsünün Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti gibi bir Azure hizmetinde kullanılması gerekir. Varsayılan değer Yanlış değeridir. `environment` ayarlandıysa, bu yok sayılır.|
| `baseImage` | `base_image` | İsteğe bağlı. Temel görüntü olarak kullanılacak özel görüntü. Hiçbir temel görüntü sağlanmazsa, görüntü, belirtilen çalışma zamanı parametresini temel alır. `environment` ayarlandıysa, bu yok sayılır. |
| `baseImageRegistry` | `base_image_registry` | İsteğe bağlı. Temel görüntüyü içeren görüntü kayıt defteri. `environment` ayarlandıysa, bu yok sayılır.|
| `cudaVersion` | `cuda_version` | İsteğe bağlı. GPU desteği gerektiren görüntüler için yüklenecek CUDA sürümü. GPU görüntüsünün Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti gibi bir Azure hizmetinde kullanılması gerekir. Desteklenen sürümler 9,0, 9,1 ve 10,0. `enable_gpu` ayarlandıysa, varsayılan değer 9,1 ' dir. `environment` ayarlandıysa, bu yok sayılır. |
| `description` | `description` | Görüntü için bir açıklama. `environment` ayarlandıysa, bu yok sayılır.  |
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

Aşağıdaki JSON, CLı ile kullanılmak üzere belirli bir sürümü olan mevcut bir Azure Machine Learning [ortamını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) kullanan örnek bir çıkarım yapılandırmadır:

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": "1"
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```

Aşağıdaki JSON, CLı ile kullanılmak üzere en son sürümü olan mevcut bir Azure Machine Learning [ortamını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) kullanan örnek bir çıkarım yapılandırmadır:

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": null
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```
