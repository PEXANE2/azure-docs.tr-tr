---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: 03b40b1f334816f08d7e169f8ff78bdb7c06c4de
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348514"
---
`inferenceconfig.json` Belgedeki girişler, [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfının parametreleriyle eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Görüntüde çalıştırılacak kodu içeren yerel dosyanın yolu. |
| `runtime` | `runtime` | Görüntü için kullanılacak çalışma zamanı. Desteklenen geçerli çalışma zamanları ' Spark-Kopyala ' ve ' Python '. |
| `condaFile` | `conda_file` | İsteğe bağlı. Görüntü için kullanılacak Conda ortam tanımını içeren yerel dosyanın yolu. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | İsteğe bağlı. Görüntü ayarlanırken çalıştırılacak ek Docker adımlarını içeren yerel dosyanın yolu. |
| `sourceDirectory` | `source_directory` | İsteğe bağlı. Görüntüyü oluşturmak için tüm dosyaları içeren klasörlerin yolu. |
| `enableGpu` | `enable_gpu` | İsteğe bağlı. Görüntüde GPU desteğinin etkinleştirilip etkinleştirilmeyeceğini belirtir. GPU görüntüsünün Microsoft Azure hizmetinde kullanılması gerekir. Örneğin, Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti. Varsayılan değer false şeklindedir. |
| `baseImage` | `base_image` | İsteğe bağlı. Temel görüntü olarak kullanılacak özel bir görüntü. Hiçbir temel görüntü verilmezse, temel görüntü belirtilen çalışma zamanı parametresi temel alınarak kullanılacaktır. |
| `baseImageRegistry` | `base_image_registry` | İsteğe bağlı. Temel görüntüyü içeren görüntü kayıt defteri. |
| `cudaVersion` | `cuda_version` | İsteğe bağlı. GPU desteği gerektiren görüntüler için yüklenecek CUDA sürümü. GPU görüntüsünün Microsoft Azure hizmetlerinde kullanılması gerekir. Örneğin, Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti. Desteklenen sürümler 9,0, 9,1 ve 10,0. ' Enable_gpu ' ayarlanırsa, varsayılan olarak ' 9,1 ' kullanılır. |
| `description` | `description` |  Bu görüntü için bir açıklama. |

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