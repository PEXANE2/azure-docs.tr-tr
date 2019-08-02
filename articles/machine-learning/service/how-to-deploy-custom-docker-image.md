---
title: Özel bir Docker görüntüsü ile modeller dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmet modellerinizi dağıttığınızda özel bir Docker görüntüsü kullanmayı öğrenin. Eğitilen bir model dağıtıldığında, görüntüyü, Web sunucusunu ve hizmeti çalıştırmak için gereken diğer bileşenleri barındırmak için bir Docker görüntüsü oluşturulur. Azure Machine Learning hizmet sizin için varsayılan bir görüntü sağladığından, kendi görüntünüzü da kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/11/2019
ms.openlocfilehash: f51c6bdc6cb2e381d5d5b855bf2f87b07d7fc180
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638431"
---
# <a name="deploy-a-model-using-a-custom-docker-image"></a>Özel bir Docker görüntüsü kullanarak model dağıtma

Eğitim modellerini Azure Machine Learning hizmeti ile dağıttığınızda özel bir Docker görüntüsü kullanmayı öğrenin.

Eğitilen bir modeli bir Web hizmetine veya IoT Edge cihaza dağıttığınızda, bir Docker görüntüsü oluşturulur. Bu görüntü modeli, Conda ortamını ve modeli kullanmak için gereken varlıkları içerir. Ayrıca, bir Web hizmeti olarak dağıtıldığında gelen istekleri işlemek için bir Web sunucusu ve Azure IoT Hub ile çalışmak için gereken bileşenleri içerir.

Azure Machine Learning hizmet varsayılan bir Docker görüntüsü sağladığından bir tane oluşturmak için endişelenmenize gerek kalmaz. Ayrıca, _temel görüntü_olarak oluşturduğunuz özel bir görüntü de kullanabilirsiniz. Bir temel görüntü, bir dağıtım için görüntü oluşturulduğunda başlangıç noktası olarak kullanılır. Temel işletim sistemi ve bileşenleri sağlar. Dağıtım işlemi daha sonra modelinize, Conda ortamı ve diğer varlıklar gibi ek bileşenleri, dağıtılmadan önce görüntüye ekler.

Genellikle, bileşen sürümlerini denetlemek veya dağıtım sırasında zaman kazanmak istediğinizde özel bir görüntü oluşturursunuz. Örneğin, belirli bir Python, Conda veya başka bir bileşen sürümünde standartlaştırmak isteyebilirsiniz. Ayrıca, modelinize gereken yazılımı yükleme işleminin uzun sürme süresini de yüklemek isteyebilirsiniz. Temel görüntü oluştururken yazılımı yüklemek, her dağıtım için yüklemeniz gerekmediği anlamına gelir.

> [!IMPORTANT]
> Bir model dağıtıldığında, Web sunucusu veya IoT Edge bileşenleri gibi temel bileşenleri geçersiz kılamazsınız. Bu bileşenler, Microsoft tarafından sınanmış ve desteklenen, bilinen bir çalışma ortamı sağlar.

> [!WARNING]
> Microsoft, özel bir görüntünün neden olduğu sorunları gidermenize yardımcı olabilir. Sorunlarla karşılaşırsanız, sorunun yansımanıza özgü olup olmadığını görmek için varsayılan görüntüyü veya Microsoft 'un sağladığı görüntülerden birini kullanmanız istenebilir.

Bu belge iki bölüme ayrılmıştır:

* Özel görüntü oluşturma: Özel görüntü oluşturma ve Azure CLı ve Machine Learning CLı kullanarak Azure Container Registry kimlik doğrulamasını yapılandırma konusunda yöneticilere ve DevOps 'a bilgi sağlar.
* Özel görüntü kullan: Python SDK veya ML CLı 'dan eğitilen bir model dağıtıldığında, özel görüntüler kullanarak veri bilimcileri ve DevOps/MLOps hakkında bilgi sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmet çalışma grubu. Daha fazla bilgi için [çalışma alanı oluşturma](setup-create-workspace.md) makalesine bakın.
* Azure Machine Learning SDK. Daha fazla bilgi için [çalışma alanı oluşturma](setup-create-workspace.md#sdk) MAKALESININ Python SDK bölümüne bakın.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Internet üzerinden erişilebilen bir [Azure Container Registry](/azure/container-registry) veya başka bir Docker kayıt defteri.
* Bu belgedeki adımlarda, model dağıtımının bir parçası olarak bir __çıkarım yapılandırma__ nesnesi oluşturma ve kullanma hakkında bilgi sahibi olduğunuz varsayılır. Daha fazla bilgi için, uygulamasının [dağıtılacağı ve nasıl yapılacağı hakkında](how-to-deploy-and-where.md#prepare-to-deploy)"dağıtmaya hazırlanma" bölümüne bakın.

## <a name="create-a-custom-image"></a>Özel görüntü oluşturma

Bu bölümdeki bilgiler, Docker görüntülerini depolamak için bir Azure Container Registry kullandığınızı varsayar. Azure Machine Learning hizmeti için özel görüntüler oluşturmayı planlarken aşağıdaki denetim listesini kullanın:

* Azure Machine Learning hizmeti çalışma alanı veya tek başına Azure Container Registry oluşturulan Azure Container Registry kullanacaksınız?

    __Çalışma alanı için kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, kayıt defterinde kimlik doğrulaması yapmanız gerekmez. Kimlik doğrulaması, çalışma alanı tarafından işlenir.

    > [!WARNING]
    > Çalışma alanınız için Azure Container Rzegistry, çalışma alanını kullanarak __bir modeli eğitmeniz veya dağıtmanız sırasında oluşturulur__ . Yeni bir çalışma alanı oluşturduysanız ancak bir model veya bir model oluşturmadıysanız, çalışma alanı için Azure Container Registry olmaz.

    Çalışma alanınızın Azure Container Registry adını alma hakkında daha fazla bilgi için bu makaledeki [kapsayıcı kayıt defteri adı alma](#getname) bölümüne bakın.

    __Tek başına kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, en azından okuma erişimi olan bir hizmet sorumlusu yapılandırmanız gerekecektir. Daha sonra, kayıt defterinden görüntüleri kullanan herkese hizmet asıl KIMLIĞI (Kullanıcı adı) ve parola sağlarsınız. Özel durum, kapsayıcı kayıt defterini herkese açık bir şekilde erişilebilir hale getirir.

    Özel Azure Container Registry oluşturma hakkında bilgi için bkz. [özel kapsayıcı kayıt defteri oluşturma](/azure/container-registry/container-registry-get-started-azure-cli).

    Azure Container Registry ile hizmet sorumlularını kullanma hakkında daha fazla bilgi için bkz. [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry ve görüntü bilgileri: Görüntü adını, kullanması gereken herkese sunun. Örneğin, adlandırılmış `myregistry`bir kayıt defterinde `myimage`depolanan adlı bir görüntü, model dağıtımı için görüntü kullanılırken `myregistry.azurecr.io/myimage` olarak başvurulur

* Görüntü gereksinimleri: Azure Machine Learning hizmeti yalnızca aşağıdaki yazılımları sağlayan Docker görüntülerini destekler:

    * Ubuntu 16,04 veya üzeri.
    * Conda 4.5. # veya üzeri.
    * Python 3.5. # veya 3.6. #.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Kapsayıcı kayıt defteri bilgilerini al

Bu bölümde, Azure Machine Learning hizmeti çalışma alanınızın Azure Container Registry adını nasıl alabileceğinizi öğrenin.

> [!WARNING]
> Çalışma alanınızın Azure Container Registry, çalışma alanını kullanarak __bir modeli eğitmeniz veya dağıtmanız için ilk kez oluşturulur__ . Yeni bir çalışma alanı oluşturduysanız ancak bir model veya bir model oluşturmadıysanız, çalışma alanı için Azure Container Registry olmaz.

Azure Machine Learning hizmetini kullanarak modeller zaten eğitilen veya dağıttıysanız, çalışma alanınız için bir kapsayıcı kayıt defteri oluşturulmuştur. Bu kapsayıcı kayıt defterinin adını bulmak için aşağıdaki adımları kullanın:

1. Yeni bir kabuk veya komut istemi açın ve Azure aboneliğinizde kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az login
    ```

    Abonelikte kimlik doğrulaması yapmak için istemleri izleyin.

2. Çalışma alanının kapsayıcı kayıt defterini listelemek için aşağıdaki komutu kullanın. Azure Machine Learning `<myworkspace>` hizmet çalışma alanınızın adıyla değiştirin. Çalışma `<resourcegroup>` alanınızı içeren Azure Kaynak grubuyla değiştirin:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    Döndürülen bilgiler aşağıdaki metne benzer:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    `<registry_name>` Değer, çalışma alanınızın Azure Container Registry adıdır.

### <a name="build-a-custom-image"></a>Özel görüntü oluşturma

Bu bölümdeki adımlar, Azure Container Registry özel bir Docker görüntüsü oluşturma konusunda yol gösterir.

1. Adlı `Dockerfile`yeni bir metin dosyası oluşturun ve içerik olarak aşağıdaki metni kullanın:

    ```text
    FROM ubuntu:16.04

    ARG CONDA_VERSION=4.5.12
    ARG PYTHON_VERSION=3.6

    ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
    ENV PATH /opt/miniconda/bin:$PATH

    RUN apt-get update --fix-missing && \
        apt-get install -y wget bzip2 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

    RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh -O ~/miniconda.sh && \
        /bin/bash ~/miniconda.sh -b -p /opt/miniconda && \
        rm ~/miniconda.sh && \
        /opt/miniconda/bin/conda clean -tipsy

    RUN conda install -y conda=${CONDA_VERSION} python=${PYTHON_VERSION} && \
        conda clean -aqy && \
        rm -rf /opt/miniconda/pkgs && \
        find / -type d -name __pycache__ -prune -exec rm -rf {} \;
    ```

2. Bir kabuktan veya komut isteminden Azure Container Registry kimlik doğrulaması yapmak için aşağıdakileri kullanın. `<registry_name>` Görüntüsünü, görüntüyü depolamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Dockerfile dosyasını karşıya yüklemek ve derlemek için aşağıdaki komutu kullanın. Görüntüyü `<registry_name>` saklamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    Yapı işlemi sırasında bilgiler komut satırına geri kaydedilir. Yapı başarılı olursa aşağıdaki metne benzer bir ileti alırsınız:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Azure Container Registry görüntü oluşturma hakkında daha fazla bilgi için bkz. [Azure Container Registry görevleri kullanarak kapsayıcı görüntüsü oluşturma ve çalıştırma](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli)

Mevcut görüntüleri bir Azure Container Registry karşıya yükleme hakkında daha fazla bilgi için, bkz. [ilk görüntünüzü özel bir Docker kapsayıcı kayıt defterine gönderme](/azure/container-registry/container-registry-get-started-docker-cli).

## <a name="use-a-custom-image"></a>Özel görüntü kullanma

Özel bir görüntü kullanmak için aşağıdaki bilgilere ihtiyacınız vardır:

* __Görüntü adı__. Örneğin, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` Microsoft tarafından sunulan temel bir Docker görüntüsünün yoludur.
* Görüntü __özel__bir depodadır, aşağıdaki bilgilere ihtiyacınız vardır:

    * Kayıt defteri __adresi__. Örneğin: `myregistry.azureecr.io`.
    * Kayıt defterine okuma erişimi olan bir hizmet sorumlusu __Kullanıcı adı__ ve __parolası__ .

    Bu bilgilere sahip değilseniz, görüntünüzü içeren Azure Container Registry için yöneticiye konuşun.

### <a name="publicly-available-images"></a>Herkese açık görüntüler

Microsoft, bu bölümdeki adımlarla kullanılabilecek, herkese açık bir şekilde erişilebilir bir depoda çeşitli Docker görüntüleri sağlar:

| Image | Açıklama |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Azure Machine Learning hizmeti için temel görüntü |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0` | ONNX çalışma zamanını içerir. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7` | ONNX çalışma zamanı ve CUDA bileşenlerini içerir. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03` | ONNX çalışma zamanını ve TensorRT 'yi içerir. |

> [!TIP]
> Bu görüntüler genel kullanıma sunulduğundan, bunları kullanırken bir adres, Kullanıcı adı veya parola sağlamanız gerekmez.

> [!IMPORTANT]
> CUDA veya TensorRT kullanan Microsoft görüntülerinin yalnızca Microsoft Azure hizmetlerinde kullanılması gerekir.

> [!TIP]
>__Modelinize Azure Machine Learning işlem üzerinde eğitim varsa__, __sürüm 1.0.22 veya__ Azure Machine Learning SDK 'sının bir üstünü kullanarak eğitim sırasında bir görüntü oluşturulur. Bu görüntünün adını saptamak için kullanın `run.properties["AzureML.DerivedImageName"]`. Aşağıdaki örnek, bu görüntünün nasıl kullanılacağını gösterir:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK ile görüntü kullanma

Özel bir görüntü kullanmak için, `base_image` [çıkarım yapılandırma nesnesinin](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) özelliğini görüntünün adresine ayarlayın:

```python
# use an image from a registry named 'myregistry'
inference_config.base_image = "myregistry.azurecr.io/myimage:v1"
```

Bu biçim, çalışma alanınız ve genel olarak erişilebilen kapsayıcı kayıt defterleri için Azure Container Registry depolanan her iki görüntü için de geçerlidir. Örneğin, aşağıdaki kod Microsoft tarafından sunulan varsayılan bir görüntü kullanır:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Çalışma alanınızda olmayan __özel bir kapsayıcı kayıt defterinden__ bir görüntü kullanmak için, deponun adresini ve Kullanıcı adını ve parolayı belirtmeniz gerekir:

```python
# Use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

### <a name="use-an-image-with-the-machine-learning-cli"></a>Machine Learning CLı ile görüntü kullanma

> [!IMPORTANT]
> Şu anda Machine Learning CLı, çalışma alanınız veya genel olarak erişilebilen depolarınız için Azure Container Registry görüntüleri kullanabilir. Tek başına özel kayıt defterlerinden görüntüleri kullanamaz.

Machine Learning CLı kullanarak bir model dağıttığınızda, Özel görüntüye başvuran bir çıkarım yapılandırma dosyası sağlarsınız. Aşağıdaki JSON belgesi, ortak kapsayıcı kayıt defterindeki bir görüntüye nasıl başvurululacağını gösterir:

```json
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "infenv.yml",
   "extraDockerfileSteps": null,
   "sourceDirectory": null,
   "enableGpu": false,
   "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
   "baseImageRegistry": "mcr.microsoft.com"
}
```

Bu dosya, `az ml model deploy` komutuyla birlikte kullanılır. `--ic` Parametresi, çıkarım yapılandırma dosyasını belirtmek için kullanılır.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

ML CLı kullanarak model dağıtma hakkında daha fazla bilgi için, [Azure Machine Learning hizmeti Için CLI uzantısının](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) "model kaydı, profil oluşturma ve dağıtım" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtım ve nasıl yapılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.
* [Azure Pipelines kullanarak makine öğrenimi modellerini eğitme ve dağıtmayı](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops)öğrenin.
