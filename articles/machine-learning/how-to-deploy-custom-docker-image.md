---
title: Özel Docker görüntüsü ile modeller dağıtma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modellerinizi dağıttığınızda özel bir Docker temel görüntüsünü nasıl kullanacağınızı öğrenin. Azure Machine Learning, sizin için varsayılan bir temel görüntü sağlar, ayrıca kendi temel görüntünüzü da kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/17/2020
ms.custom: tracking-python
ms.openlocfilehash: aa55285a2447f9baadfc74782a46bcd6c68b545e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87012289"
---
# <a name="deploy-a-model-using-a-custom-docker-base-image"></a>Özel bir Docker temel görüntüsü kullanarak model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ile eğitilen modeller dağıttığınızda özel bir Docker temel görüntüsünü nasıl kullanacağınızı öğrenin.

Eğitilen bir modeli bir Web hizmetine veya IoT Edge cihaza dağıttığınızda, gelen istekleri işlemek için bir Web sunucusu içeren bir paket oluşturulur.

Azure Machine Learning, bir varsayılan Docker temel görüntüsü sağlar, bu sayede bir tane oluşturmak için endişelenmenize gerek kalmaz. Ayrıca, belirli bir temel görüntü seçmek için Azure Machine Learning __ortamları__ kullanabilir veya sağladığınız özel bir tane kullanabilirsiniz.

Bir temel görüntü, bir dağıtım için görüntü oluşturulduğunda başlangıç noktası olarak kullanılır. Temel işletim sistemi ve bileşenleri sağlar. Dağıtım işlemi daha sonra modelinize, Conda ortamı ve diğer varlıklar gibi ek bileşenleri, dağıtılmadan önce görüntüye ekler.

Genellikle, bağımlılıklarınızı yönetmek için Docker 'ı kullanmak istediğinizde, bileşen sürümleri üzerinde daha sıkı denetim sağlamak veya dağıtım sırasında zamandan tasarruf etmek istediğinizde özel bir temel görüntü oluşturursunuz. Örneğin, belirli bir Python, Conda veya başka bir bileşen sürümünde standartlaştırmak isteyebilirsiniz. Ayrıca, modelinize gereken yazılımı yükleme işleminin uzun sürme süresini de yüklemek isteyebilirsiniz. Temel görüntü oluştururken yazılımı yüklemek, her dağıtım için yüklemeniz gerekmediği anlamına gelir.

> [!IMPORTANT]
> Bir modeli dağıtırken, Web sunucusu veya IoT Edge bileşenleri gibi temel bileşenleri geçersiz kılamazsınız. Bu bileşenler, Microsoft tarafından sınanmış ve desteklenen, bilinen bir çalışma ortamı sağlar.

> [!WARNING]
> Microsoft, özel bir görüntünün neden olduğu sorunları gidermenize yardımcı olabilir. Sorunlarla karşılaşırsanız, sorunun yansımanıza özgü olup olmadığını görmek için varsayılan görüntüyü veya Microsoft 'un sağladığı görüntülerden birini kullanmanız istenebilir.

Bu belge iki bölüme ayrılmıştır:

* Özel bir temel görüntü oluşturma: özel görüntü oluşturma ve Azure CLı ve Machine Learning CLı kullanarak Azure Container Registry kimlik doğrulamasını yapılandırma konusunda yöneticilere ve DevOps 'a bilgi sağlar.
* Özel bir temel görüntü kullanarak bir model dağıtma: Python SDK veya ML CLı 'dan eğitilen bir model dağıtırken özel görüntüler kullanma hakkında veri bilimcileri ve DevOps/ML mühendislerine yönelik bilgiler sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning çalışma grubu. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.
* [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). 
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Internet üzerinden erişilebilen bir [Azure Container Registry](/azure/container-registry) veya başka bir Docker kayıt defteri.
* Bu belgedeki adımlarda, model dağıtımının bir parçası olarak bir __çıkarım yapılandırma__ nesnesi oluşturma ve kullanma hakkında bilgi sahibi olduğunuz varsayılır. Daha fazla bilgi için, uygulamasının [dağıtılacağı ve nasıl yapılacağı hakkında](how-to-deploy-and-where.md#prepare-to-deploy)"dağıtmaya hazırlanma" bölümüne bakın.

## <a name="create-a-custom-base-image"></a>Özel bir temel görüntü oluşturma

Bu bölümdeki bilgiler, Docker görüntülerini depolamak için bir Azure Container Registry kullandığınızı varsayar. Azure Machine Learning için özel görüntüler oluşturmayı planlarken aşağıdaki denetim listesini kullanın:

* Azure Machine Learning çalışma alanı veya tek başına Azure Container Registry oluşturulan Azure Container Registry kullanacaksınız?

    __Çalışma alanı için kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, kayıt defterinde kimlik doğrulaması yapmanız gerekmez. Kimlik doğrulaması, çalışma alanı tarafından işlenir.

    > [!WARNING]
    > Çalışma alanınızın Azure Container Registry, çalışma alanını kullanarak __bir modeli eğitmeniz veya dağıtmanız için ilk kez oluşturulur__ . Yeni bir çalışma alanı oluşturduysanız ancak bir model veya bir model oluşturmadıysanız, çalışma alanı için Azure Container Registry olmaz.

    Çalışma alanınızın Azure Container Registry adını alma hakkında daha fazla bilgi için bu makaledeki [kapsayıcı kayıt defteri adı alma](#getname) bölümüne bakın.

    __Tek başına kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, en azından okuma erişimi olan bir hizmet sorumlusu yapılandırmanız gerekecektir. Daha sonra, kayıt defterinden görüntüleri kullanan herkese hizmet asıl KIMLIĞI (Kullanıcı adı) ve parola sağlarsınız. Özel durum, kapsayıcı kayıt defterini herkese açık bir şekilde erişilebilir hale getirir.

    Özel Azure Container Registry oluşturma hakkında bilgi için bkz. [özel kapsayıcı kayıt defteri oluşturma](/azure/container-registry/container-registry-get-started-azure-cli).

    Azure Container Registry ile hizmet sorumlularını kullanma hakkında daha fazla bilgi için bkz. [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry ve görüntü bilgileri: görüntü adını, kullanması gereken herkese sunun. Örneğin, `myimage` adlandırılmış bir kayıt defterinde depolanan adlı bir görüntü, `myregistry` `myregistry.azurecr.io/myimage` model dağıtımı için görüntü kullanılırken olarak başvurulur

* Görüntü gereksinimleri: Azure Machine Learning yalnızca aşağıdaki yazılımları sağlayan Docker görüntülerini destekler:

    * Ubuntu 16,04 veya üzeri.
    * Conda 4.5. # veya üzeri.
    * Python 3.5. #, 3.6. # veya 3.7. #.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Kapsayıcı kayıt defteri bilgilerini al

Bu bölümde, Azure Machine Learning çalışma alanınızın Azure Container Registry adını nasıl alabileceğinizi öğrenin.

> [!WARNING]
> Çalışma alanınızın Azure Container Registry, çalışma alanını kullanarak __bir modeli eğitmeniz veya dağıtmanız için ilk kez oluşturulur__ . Yeni bir çalışma alanı oluşturduysanız ancak bir model veya bir model oluşturmadıysanız, çalışma alanı için Azure Container Registry olmaz.

Azure Machine Learning kullanarak modeller zaten eğitimişseniz veya dağıttıysanız, çalışma alanınız için bir kapsayıcı kayıt defteri oluşturulmuştur. Bu kapsayıcı kayıt defterinin adını bulmak için aşağıdaki adımları kullanın:

1. Yeni bir kabuk veya komut istemi açın ve Azure aboneliğinizde kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az login
    ```

    Abonelikte kimlik doğrulaması yapmak için istemleri izleyin.

    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

2. Çalışma alanının kapsayıcı kayıt defterini listelemek için aşağıdaki komutu kullanın. `<myworkspace>`Azure Machine Learning çalışma alanınızın adıyla değiştirin. `<resourcegroup>`Çalışma alanınızı Içeren Azure Kaynak grubuyla değiştirin:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

    Döndürülen bilgiler aşağıdaki metne benzer:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    `<registry_name>`Değer, çalışma alanınızın Azure Container Registry adıdır.

### <a name="build-a-custom-base-image"></a>Özel bir temel görüntü oluşturma

Bu bölümdeki adımlar, Azure Container Registry özel bir Docker görüntüsü oluşturma konusunda yol gösterir.

1. Adlı yeni bir metin dosyası oluşturun `Dockerfile` ve içerik olarak aşağıdaki metni kullanın:

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

2. Bir kabuktan veya komut isteminden Azure Container Registry kimlik doğrulaması yapmak için aşağıdakileri kullanın. Görüntüsünü, `<registry_name>` görüntüyü depolamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Dockerfile dosyasını karşıya yüklemek ve derlemek için aşağıdaki komutu kullanın. `<registry_name>`Görüntüyü saklamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    > [!TIP]
    > Bu örnekte, bir etiketi `:v1` görüntüye uygulanır. Hiçbir etiket sağlanmazsa, bir etiketi `:latest` uygulanır.

    Yapı işlemi sırasında bilgiler komut satırına geri kaydedilir. Yapı başarılı olursa aşağıdaki metne benzer bir ileti alırsınız:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Azure Container Registry görüntü oluşturma hakkında daha fazla bilgi için bkz. [Azure Container Registry görevleri kullanarak kapsayıcı görüntüsü oluşturma ve çalıştırma](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli)

Mevcut görüntüleri bir Azure Container Registry karşıya yükleme hakkında daha fazla bilgi için, bkz. [ilk görüntünüzü özel bir Docker kapsayıcı kayıt defterine gönderme](/azure/container-registry/container-registry-get-started-docker-cli).

## <a name="use-a-custom-base-image"></a>Özel bir temel görüntü kullanma

Özel bir görüntü kullanmak için aşağıdaki bilgilere ihtiyacınız vardır:

* __Görüntü adı__. Örneğin, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda:latest` Microsoft tarafından sunulan temel bir Docker görüntüsünün yoludur.

    > [!IMPORTANT]
    > Oluşturduğunuz özel görüntüler için görüntüyle birlikte kullanılan tüm etiketleri eklediğinizden emin olun. Örneğin, görüntünüz gibi belirli bir etiketle oluşturulduysa `:v1` . Görüntüyü oluştururken belirli bir etiket kullanmıyorsanız, bir etiketi `:latest` uygulandı.

* Görüntü __özel bir depodadır__, aşağıdaki bilgilere ihtiyacınız vardır:

    * Kayıt defteri __adresi__. Örneğin, `myregistry.azureecr.io`.
    * Kayıt defterine okuma erişimi olan bir hizmet sorumlusu __Kullanıcı adı__ ve __parolası__ .

    Bu bilgilere sahip değilseniz, görüntünüzü içeren Azure Container Registry için yöneticiye konuşun.

### <a name="publicly-available-base-images"></a>Genel kullanıma açık temel görüntüler

Microsoft, bu bölümdeki adımlarla kullanılabilecek, herkese açık bir şekilde erişilebilir bir depoda çeşitli Docker görüntüleri sağlar:

| Görüntü | Açıklama |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Azure Machine Learning için temel görüntü |
| `mcr.microsoft.com/azureml/onnxruntime:latest` | CPU ınzoni için ONNX çalışma zamanını içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-cuda` | GPU için ONNX çalışma zamanını ve CUDA 'yı içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-tensorrt` | GPU için ONNX çalışma zamanını ve TensorRT 'yi içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-vadm ` | <sup></sup>Movidius<sup>TM</sup> myriadx Vpus tabanlı Intel Vision Hızlandırıcısı tasarımı Için Onnx çalışma zamanı ve openvino içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-myriad` | Intel <sup></sup> Movidius<sup>TM</sup> USB etiketleri Için Onnx çalışma zamanı ve openvino içerir |

ONNX çalışma zamanı temel görüntüleri hakkında daha fazla bilgi için GitHub deposu 'ndaki [Onnx Runtime dockerfile bölümüne](https://github.com/microsoft/onnxruntime/blob/master/dockerfiles/README.md) bakın.

> [!TIP]
> Bu görüntüler genel kullanıma sunulduğundan, bunları kullanırken bir adres, Kullanıcı adı veya parola sağlamanız gerekmez.

Daha fazla bilgi için bkz. GitHub 'da [Azure Machine Learning kapsayıcıları](https://github.com/Azure/AzureML-Containers) deposu.

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK ile görüntü kullanma

**Çalışma alanınızın Azure Container Registry**depolanan bir görüntüyü veya genel olarak **erişilebilen bir kapsayıcı kayıt defterini**kullanmak için aşağıdaki [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) özniteliklerini ayarlayın:

+ `docker.enabled=True`
+ `docker.base_image`: Görüntünün kayıt defterine ve yoluna ayarlanır.

```python
from azureml.core.environment import Environment
# Create the environment
myenv = Environment(name="myenv")
# Enable Docker and reference an image
myenv.docker.enabled = True
myenv.docker.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda:latest"
```

Çalışma alanınızda olmayan __özel bir kapsayıcı kayıt defterinden__ bir görüntü kullanmak için, `docker.base_image_registry` deponun adresini ve Kullanıcı adını ve parolayı belirtmek için öğesini kullanmanız gerekir:

```python
# Set the container registry information
myenv.docker.base_image_registry.address = "myregistry.azurecr.io"
myenv.docker.base_image_registry.username = "username"
myenv.docker.base_image_registry.password = "password"

myenv.inferencing_stack_version = "latest"  # This will install the inference specific apt packages.

# Define the packages needed by the model and scripts
from azureml.core.conda_dependencies import CondaDependencies
conda_dep = CondaDependencies()
# you must list azureml-defaults as a pip dependency
conda_dep.add_pip_package("azureml-defaults")
myenv.python.conda_dependencies=conda_dep
```

Bir PIP bağımlılığı olarak >= 1.0.45 sürümü ile azureml-varsayılanlar eklemeniz gerekir. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerir. Ayrıca, ortamda inferencing_stack_version özelliğini "en son" olarak ayarlamanız gerekir, bu, Web hizmeti tarafından gereken belirli apt paketlerini yükler. 

Ortamı tanımladıktan sonra, model ve Web hizmeti 'nin çalışacağı çıkarım ortamını tanımlamak için bunu bir [ınısenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) nesnesiyle birlikte kullanın.

```python
from azureml.core.model import InferenceConfig
# Use environment in InferenceConfig
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
```

Bu noktada, dağıtıma devam edebilirsiniz. Örneğin, aşağıdaki kod parçacığı, çıkarım yapılandırması ve özel görüntü kullanarak bir Web hizmetini yerel olarak dağıtır:

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Dağıtım hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

Python ortamınızı özelleştirme hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md). 

### <a name="use-an-image-with-the-machine-learning-cli"></a>Machine Learning CLı ile görüntü kullanma

> [!IMPORTANT]
> Şu anda Machine Learning CLı, çalışma alanınız veya genel olarak erişilebilen depolarınız için Azure Container Registry görüntüleri kullanabilir. Tek başına özel kayıt defterlerinden görüntüleri kullanamaz.

Machine Learning CLı kullanarak bir modeli dağıtmadan önce, özel görüntüyü kullanan bir [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) oluşturun. Ardından, ortama başvuran bir çıkarım yapılandırma dosyası oluşturun. Ortamı doğrudan çıkarım yapılandırma dosyasında da tanımlayabilirsiniz. Aşağıdaki JSON belgesi, ortak kapsayıcı kayıt defterindeki bir görüntüye nasıl başvurululacağını gösterir. Bu örnekte, ortam satır içi olarak tanımlanmıştır:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda:latest",
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

Bu dosya, komutuyla birlikte kullanılır `az ml model deploy` . `--ic`Parametresi, çıkarım yapılandırma dosyasını belirtmek için kullanılır.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

ML CLı kullanarak model dağıtma hakkında daha fazla bilgi için, [Azure Machine Learning Için CLI uzantısının](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) "model kaydı, profil oluşturma ve dağıtım" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtım ve nasıl yapılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.
* [Azure Pipelines kullanarak makine öğrenimi modellerini eğitme ve dağıtmayı](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops)öğrenin.
