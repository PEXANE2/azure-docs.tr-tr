---
title: Özel bir Docker görüntüsü kullanarak bir modeli eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 'da özel Docker görüntüleriyle modelleri eğitme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sagopal
author: saachigopal
ms.date: 08/11/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 2289a761d4e266c305c2868e9f234871624ae528
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661313"
---
# <a name="train-a-model-using-a-custom-docker-image"></a>Özel bir Docker görüntüsü kullanarak bir modeli eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning modelleriyle eğitim yaparken özel bir Docker görüntüsü kullanmayı öğrenin. 

Bu makaledeki örnek betikler, bir evsel sinir ağı oluşturarak Evcil hayvan görüntülerini sınıflandırmak için kullanılır. 

Azure Machine Learning varsayılan bir Docker temel görüntüsü sağlarken, korunan [Azure ML taban görüntülerinin](https://github.com/Azure/AzureML-Containers) veya kendi [özel görüntünüzün](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-custom-docker-image#create-a-custom-base-image)bir kümesi gibi belirli bir temel görüntü belirtmek için Azure Machine Learning ortamlarını de kullanabilirsiniz. Özel temel görüntüler, kendi bağımlılıklarınızı yakından yönetmenize ve eğitim işleri yürütürken bileşen sürümleri üzerinde daha sıkı bir denetim sürdürmenize imkan tanır. 

## <a name="prerequisites"></a>Ön koşullar 
Bu kodu şu ortamlardan birinde çalıştırın:
* Azure Machine Learning işlem örneği-indirme veya yükleme gerekli değil
    * Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için [ortamı ve çalışma alanını kurma](tutorial-1st-experiment-sdk-setup.md) .
    * Azure Machine Learning [örnekleri deposunda](https://github.com/Azure/azureml-examples), bu dizine giderek tamamlanan bir not defteri bulun: **Not defterleri > fastai > tren-pets-resnet34. ipynb** 

* Kendi Jupyter Notebook sunucunuz
    * Bir [çalışma alanı yapılandırma dosyası](how-to-configure-environment.md#workspace)oluşturun.
    * [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true). 
    * Internet üzerinden erişilebilen bir [Azure Container Registry](/azure/container-registry) veya başka bir Docker kayıt defteri.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama 
Bu bölüm, bir çalışma alanını başlatarak, deneme oluşturarak ve eğitim verilerini ve eğitim betiklerini karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma
[Azure Machine Learning çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) .

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="prepare-scripts"></a>Betikleri hazırlama
Bu öğretici için eğitim betiği **train.py** [burada](https://github.com/Azure/azureml-examples/blob/main/code/models/fastai/pets-resnet34/train.py)verilmiştir. Uygulamada, tüm özel eğitim komut dosyalarını olduğu gibi alabilir ve Azure Machine Learning ile çalıştırabilirsiniz.

### <a name="define-your-environment"></a>Ortamınızı tanımlama
Bir ortam nesnesi oluşturun ve Docker 'ı etkinleştirin. 

```python
from azureml.core import Environment

fastai_env = Environment("fastai2")
fastai_env.docker.enabled = True
```

Bu belirtilen temel görüntüde, dağıtılmış derin öğrenme özelliklerine izin veren fast.ai kitaplığı desteklenir. Daha fazla bilgi için bkz. [Fast.ai DockerHub](https://hub.docker.com/u/fastdotai). 

Özel Docker görüntünüzü kullanırken, zaten Python ortamınız ayarlanmış olabilir. Bu durumda, `user_managed_dependencies` özel görüntünüzün yerleşik Python ortamını kullanabilmesi için bayrağını True olarak ayarlayın. Varsayılan olarak, Azure ML belirttiğiniz bağımlılıklara sahip bir Conda ortamı oluşturur ve temel görüntüde yüklü olan herhangi bir Python kitaplıklarını kullanmak yerine bu ortamda çalışmayı yürütür.

```python
fastai_env.docker.base_image = "fastdotai/fastai2:latest"
fastai_env.python.user_managed_dependencies = True
```

Çalışma alanınızda olmayan özel bir kapsayıcı kayıt defterinden bir görüntü kullanmak için, `docker.base_image_registry` deponun adresini ve Kullanıcı adını ve parolayı belirtmek için öğesini kullanmanız gerekir:

```python
# Set the container registry information
fastai_env.docker.base_image_registry.address = "myregistry.azurecr.io"
fastai_env.docker.base_image_registry.username = "username"
fastai_env.docker.base_image_registry.password = "password"
```

Özel bir Dockerfile kullanmak da mümkündür. Python olmayan paketleri bağımlılıklar olarak yüklemeniz ve temel görüntüyü None olarak ayarlamayı hatırlamak istiyorsanız bu yaklaşımı kullanın.

```python 
# Specify docker steps as a string. 
dockerfile = r"""
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
fastai_env.docker.base_image = None
fastai_env.docker.base_dockerfile = dockerfile

# Alternatively, load the string from a file.
fastai_env.docker.base_image = None
fastai_env.docker.base_dockerfile = "./Dockerfile"
```

### <a name="create-or-attach-existing-amlcompute"></a>Mevcut AmlCompute oluşturun veya ekleyin
Modelinize eğitim için bir [işlem hedefi](https://docs.microsoft.com/azure/machine-learning/concept-azure-machine-learning-architecture#compute-target) oluşturmanız gerekecektir. Bu öğreticide, eğitim işlem kaynağınız olarak AmlCompute oluşturursunuz.

AmlCompute oluşturma işlemi yaklaşık 5 dakika sürer. Bu ada sahip AmlCompute zaten çalışma alanınızda varsa, bu kod oluşturma işlemini atlar.

Diğer Azure hizmetlerinde olduğu gibi, Azure Machine Learning hizmetiyle ilişkili belirli kaynaklarda (ör. AmlCompute) sınırlamalar vardır. Lütfen [Bu makaleyi](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas) varsayılan sınırlarda okuyun ve nasıl daha fazla kota isteneceğini öğrenin. 

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current AmlCompute
print(compute_target.get_status().serialize())
```

### <a name="create-a-scriptrunconfig"></a>ScriptRunConfig oluşturma
Bu ScriptRunConfig, işinizi istenen [işlem hedefinde](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets#compute-targets-for-training)yürütmek üzere yapılandırır.

```python
from azureml.core import ScriptRunConfig

fastai_config = ScriptRunConfig(source_directory='fastai-example', script='train.py')
fastai_config.run_config.environment = fastai_env
fastai_config.run_config.target = compute_target
```

### <a name="submit-your-run"></a>Çalıştırmanızı gönder
Bir eğitim çalıştırması bir ScriptRunConfig nesnesi kullanılarak gönderildiğinde, Gönder yöntemi ScriptRun türünde bir nesne döndürür. Döndürülen ScriptRun nesnesi, eğitim çalıştırmaları hakkında bilgi için programlı erişim sağlar. 

```python
from azureml.core import Experiment

run = Experiment(ws,'fastai-custom-image').submit(fastai_config)
run.wait_for_completion(show_output=True)
```

> [!WARNING]
> Azure Machine Learning, tüm kaynak dizinini kopyalayarak eğitim betikleri çalıştırır. Karşıya yüklemek istemediğiniz gizli verileriniz varsa, bir [. Ignore dosyası](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots) kullanın veya kaynak dizine eklemeyin. Bunun yerine, veri [deposu](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py&preserve-view=true)kullanarak verilerinize erişin.

Python ortamınızı özelleştirme hakkında daha fazla bilgi için bkz. [oluşturma & yazılım ortamları kullanma](how-to-use-environments.md). 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, özel bir Docker görüntüsü kullanarak bir model eğitiliniz. Azure Machine Learning hakkında daha fazla bilgi edinmek için bu makaleye bakın.
* Eğitim sırasında [çalıştırma ölçümlerini izleyin](how-to-track-experiments.md)
* Özel bir Docker görüntüsü kullanarak [bir model dağıtın](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-custom-docker-image) .
