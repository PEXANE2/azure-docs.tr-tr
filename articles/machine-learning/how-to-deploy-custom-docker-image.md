---
title: Özel Docker görüntüsüne sahip modelleri dağıtma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modellerinizi dağıtırken özel Docker taban görüntüsünü nasıl kullanacağınızı öğrenin. Azure Machine Learning sizin için varsayılan bir temel görüntü sağlarken, kendi temel resminizi de kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 03/16/2020
ms.openlocfilehash: a237beb72e35a236e353c58db520a8d611fdfdcd
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617994"
---
# <a name="deploy-a-model-using-a-custom-docker-base-image"></a>Özel Docker taban görüntüsünü kullanarak bir model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ile eğitimli modelleri dağıtırken özel docker taban görüntüsünü nasıl kullanacağınızı öğrenin.

Bir web hizmetine veya IoT Edge aygıtına eğitilmiş bir model dağıttığınızda, gelen istekleri işlemek için bir web sunucusu içeren bir paket oluşturulur.

Azure Machine Learning varsayılan docker temel görüntüsü sağlar, böylece bir tane oluşturma konusunda endişelenmenize gerek kalmaz. Azure Machine Learning __ortamlarını__ belirli bir temel resmi seçmek veya sağladığınız özel bir resim kullanmak için de kullanabilirsiniz.

Bir dağıtım için görüntü oluşturulduğunda, temel görüntü başlangıç noktası olarak kullanılır. Bu altta yatan işletim sistemi ve bileşenleri sağlar. Dağıtım işlemi daha sonra görüntüye dağıtmadan önce modeliniz, conda ortamınız ve diğer varlıklarınız gibi ek bileşenler ekler.

Genellikle, bağımlılıklarınızı yönetmek, bileşen sürümleri üzerinde daha sıkı denetim sağlamak veya dağıtım sırasında zamandan tasarruf etmek için Docker'ı kullanmak istediğinizde özel bir temel görüntü oluşturursunuz. Örneğin, Python, Conda veya diğer bileşenin belirli bir sürümünde standartlaştırmak isteyebilirsiniz. Yükleme işleminin uzun zaman aldığı modelin gerektirdiği yazılımı da yüklemek isteyebilirsiniz. Temel görüntüyü oluştururken yazılımı yüklemek, her dağıtım için yüklemeniz gerekmediğiniz anlamına gelir.

> [!IMPORTANT]
> Bir model dağıttığınızda, web sunucusu veya IoT Edge bileşenleri gibi temel bileşenleri geçersiz kılamazsınız. Bu bileşenler, Microsoft tarafından sınanan ve desteklenen bilinen bir çalışma ortamı sağlar.

> [!WARNING]
> Microsoft, özel bir görüntünün neden olduğu sorunları gidermeye yardımcı olmayabilir. Sorunlarla karşılaşırsanız, sorunun resminize özgü olup olmadığını görmek için varsayılan görüntüyü veya Microsoft'un sağladığı resimlerden birini kullanmanız istenebilir.

Bu belge iki bölüme ayrılmıştır:

* Özel bir temel görüntü oluşturma: Yöneticilere ve DevOp'lara azure CLI ve Machine Learning CLI'yi kullanarak özel bir resim oluşturma ve kimlik doğrulamayı bir Azure Kapsayıcı Kayıt Defteri'ne yapılandırma hakkında bilgi sağlar.
* Özel bir temel görüntü kullanarak bir model dağıtın: Python SDK veya ML CLI'den eğitilmiş bir modeli dağıtırken özel görüntüler kullanma konusunda Veri Bilimcileri ve DevOps / ML Mühendislerine bilgi sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma grubu. Daha fazla bilgi için [çalışma alanı](how-to-manage-workspace.md) oluştur makalesine bakın.
* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). 
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning için CLI uzantısı.](reference-azure-machine-learning-cli.md)
* Azure [Konteyner Kayıt Defteri](/azure/container-registry) veya Internet üzerinden erişilebilen diğer Docker kayıt defteri.
* Bu belgedeki adımlar, model dağıtımının bir parçası olarak __çıkarım yapılandırma__ nesnesi oluşturma ve kullanma aşina olduğunuzu varsayar. Daha fazla bilgi için, nereye ve [nasıl dağıtılanın](how-to-deploy-and-where.md#prepare-to-deploy)"dağıtmaya hazırlanın" bölümüne bakın.

## <a name="create-a-custom-base-image"></a>Özel bir temel görüntü oluşturma

Bu bölümdeki bilgiler, Docker görüntülerini depolamak için bir Azure Kapsayıcı Kayıt Defteri kullandığınızı varsayar. Azure Machine Learning için özel görüntüler oluşturmayı planlarken aşağıdaki denetim listesini kullanın:

* Azure Machine Learning çalışma alanı için oluşturulan Azure Kapsayıcı Kayıt Defteri'ni mi yoksa bağımsız bir Azure Kapsayıcı Kayıt Defteri'ni mi kullanacaksınız?

    __Çalışma alanı için kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, kayıt defterine kimlik doğrulamanız gerekmez. Kimlik doğrulaması çalışma alanı tarafından işlenir.

    > [!WARNING]
    > Çalışma alanınız için Azure Kapsayıcı Kayıt Defteri, çalışma alanını kullanarak __bir modeli ilk kez eğittiğınızda veya dağıttığınızda oluşturulur.__ Yeni bir çalışma alanı oluşturduysanız, ancak eğitilmediyseniz veya bir model oluşturduysanız, çalışma alanı için hiçbir Azure Kapsayıcı Kayıt Defteri bulunmaz.

    Çalışma alanınız için Azure Kapsayıcı Kayıt Defteri'nin adını alma hakkında bilgi için bu makalenin [kapsayıcı kayıt defteri adı](#getname) bölümünü görün.

    Bağımsız bir __kapsayıcı kayıt defterinde__depolanan görüntüleri kullanırken, en azından erişim okunan bir hizmet ilkesini yapılandırmanız gerekir. Daha sonra, kayıt defterindeki resimleri kullanan herkese hizmet ana kimliği (kullanıcı adı) ve parola sağlarsınız. Bunun istisnası, kapsayıcı kayıt defterini herkese açık hale getirmenizdir.

    Özel bir Azure Kapsayıcı Kayıt Defteri oluşturma hakkında bilgi [için](/azure/container-registry/container-registry-get-started-azure-cli)bkz.

    Azure Kapsayıcı Kayıt Defteri'nde hizmet ilkelerini kullanma hakkında daha fazla bilgi [için, hizmet ilkeleriyle birlikte Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması'na](/azure/container-registry/container-registry-auth-service-principal)bakın.

* Azure Kapsayıcı Kayıt Defteri ve görüntü bilgileri: Görüntü adını kullanması gereken herkese sağlayın. Örneğin, model dağıtımı `myimage`için görüntüyü kullanırken `myregistry`olarak `myregistry.azurecr.io/myimage` başvurulan , adlı bir kayıt defterinde depolanan bir resim

* Görüntü gereksinimleri: Azure Machine Learning yalnızca aşağıdaki yazılımı sağlayan Docker görüntülerini destekler:

    * Ubuntu 16.04 veya daha büyük.
    * Conda 4.5.# veya daha büyük.
    * Python 3.5.# veya 3.6.#.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Konteyner kayıt defteri bilgilerini alma

Bu bölümde, Azure Makine Öğrenimi çalışma alanınız için Azure Konteyner Kayıt Defteri'nin adını nasıl alacağınızı öğrenin.

> [!WARNING]
> Çalışma alanınız için Azure Kapsayıcı Kayıt Defteri, çalışma alanını kullanarak __bir modeli ilk kez eğittiğınızda veya dağıttığınızda oluşturulur.__ Yeni bir çalışma alanı oluşturduysanız, ancak eğitilmediyseniz veya bir model oluşturduysanız, çalışma alanı için hiçbir Azure Kapsayıcı Kayıt Defteri bulunmaz.

Azure Machine Learning'i kullanarak modelleri zaten eğittiyseniz veya dağıttıysanız, çalışma alanınız için bir konteyner kayıt defteri oluşturuldu. Bu kapsayıcı kayıt defterinin adını bulmak için aşağıdaki adımları kullanın:

1. Yeni bir kabuk veya komut istemi açın ve Azure aboneliğinizde kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az login
    ```

    Aboneliğin kimliğini doğrulamak için istemleri izleyin.

    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

2. Çalışma alanı için kapsayıcı kayıt defterini listelemek için aşağıdaki komutu kullanın. Azure `<myworkspace>` Machine Learning çalışma alanı adınızı değiştirin. Çalışma `<resourcegroup>` alanınızı içeren Azure kaynak grubuyla değiştirin:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

    Döndürülen bilgiler aşağıdaki metne benzer:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    Değer, `<registry_name>` çalışma alanınız için Azure Kapsayıcı Kayıt Defteri'nin adıdır.

### <a name="build-a-custom-base-image"></a>Özel bir temel görüntü oluşturma

Bu bölümdeki adımlar, Azure Konteyner Kayıt Defterinizde özel bir Docker görüntüsü oluşturmada geçer.

1. Adlı `Dockerfile`yeni bir metin dosyası oluşturun ve aşağıdaki metni içerik olarak kullanın:

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

2. Bir kabuk veya komut isteminden, Azure Kapsayıcı Kayıt Defteri'nin kimliğini doğrulamak için aşağıdakileri kullanın. `<registry_name>` Görüntüyü depolamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Dockerfile'ı yüklemek ve oluşturmak için aşağıdaki komutu kullanın. Görüntüyü `<registry_name>` depolamak istediğiniz kapsayıcı kayıt defterinin adıyla değiştirin:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    > [!TIP]
    > Bu örnekte, görüntüye bir etiket `:v1` uygulanır. Etiket `:latest` sağlanmazsa, bir etiket uygulanır.

    Oluşturma işlemi sırasında, bilgiler komut satırına geri akışı. Yapı başarılı olursa, aşağıdaki metne benzer bir ileti alırsınız:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Azure Kapsayıcı Kayıt Defteri ile resim oluşturma hakkında daha fazla bilgi için Azure [Kapsayıcı Kayıt Defteri Görevlerini kullanarak kapsayıcı görüntüsü oluştur ve çalıştır'a](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli) bakın

Varolan görüntüleri bir Azure Kapsayıcı Kayıt Defterine yükleme hakkında daha fazla bilgi için [bkz.](/azure/container-registry/container-registry-get-started-docker-cli)

## <a name="use-a-custom-base-image"></a>Özel bir temel görüntü kullanma

Özel bir görüntü kullanmak için aşağıdaki bilgilere ihtiyacınız vardır:

* __Resim adı.__ Örneğin, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` Microsoft tarafından sağlanan temel bir Docker Image'a giden yoldur.

    > [!IMPORTANT]
    > Oluşturduğunuz özel görüntüler için, görüntüyle birlikte kullanılan etiketleri eklediğinizden emin olun. Örneğin, resminiz . `:v1` Görüntüyü oluştururken belirli bir etiket kullanmadıysanız, bir `:latest` etiket uygulandı.

* Görüntü özel bir __depodaysa,__ aşağıdaki bilgilere ihtiyacınız var:

    * Kayıt defteri __adresi.__ Örneğin, `myregistry.azureecr.io`.
    * Kayıt defterine erişim okunan bir hizmet temel __kullanıcı adı__ ve __parola.__

    Bu bilgilere sahip değilseniz, resminizi içeren Azure Kapsayıcı Kayıt Defteri için yöneticiyle konuşun.

### <a name="publicly-available-base-images"></a>Genel kullanıma açık temel görüntüler

Microsoft, bu bölümdeki adımlarla kullanılabilen, herkese açık bir depoda birkaç docker görüntüsü sağlar:

| Görüntü | Açıklama |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Azure Machine Learning için temel resim |
| `mcr.microsoft.com/azureml/onnxruntime:latest` | CPU çıkartma için ONNX Runtime içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-cuda` | GPU için ONNX Çalışma Süresi ve CUDA içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-tensorrt` | GPU için ONNX Çalışma Süresi ve TensorRT içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-vadm ` | Movidius<sup>TM</sup> MyriadX<sup> </sup> VPUs dayalı Intel Vision Hızlandırıcı Tasarım için ONNX Çalışma Süresi ve OpenVINO içerir |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-myriad` | Intel<sup> </sup> Movidius<sup>TM</sup> USB çubukları için ONNX Çalışma Süresi ve OpenVINO içerir |

ONNX Runtime temel görüntüleri hakkında daha fazla bilgi için GitHub repo'daki [ONNX Runtime dockerfile bölümüne](https://github.com/microsoft/onnxruntime/blob/master/dockerfiles/README.md) bakın.

> [!TIP]
> Bu resimler herkese açık olduğundan, bunları kullanırken bir adres, kullanıcı adı veya parola sağlamanız gerekmez.

Daha fazla bilgi için Azure [Machine Learning kapsayıcıları'na](https://github.com/Azure/AzureML-Containers)bakın.

> [!TIP]
>__Modeliniz Azure Machine Learning Compute'da__ __1.0.22 veya daha büyük azure__ machine learning sdk sürümü kullanılarak eğitildiyse, eğitim sırasında bir resim oluşturulur. Bu resmin adını bulmak `run.properties["AzureML.DerivedImageName"]`için . Aşağıdaki örnek, bu görüntünün nasıl kullanılacağını gösterir:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK ile görüntü kullanma

**Çalışma alanınız için Azure Kapsayıcı Kayıt Defteri'nde**depolanan bir resmi veya herkese açık bir **kapsayıcı kayıt defterini**kullanmak için aşağıdaki [Çevre](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) özniteliklerini ayarlayın:

+ `docker.enabled=True`
+ `docker.base_image`: Kayıt defterine ve görüntüye giden yola ayarlayın.

```python
from azureml.core.environment import Environment
# Create the environment
myenv = Environment(name="myenv")
# Enable Docker and reference an image
myenv.docker.enabled = True
myenv.docker.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Çalışma alanınızda olmayan özel bir __kapsayıcı kayıt defterinden__ bir resim `docker.base_image_registry` kullanmak için, deponun adresini ve kullanıcı adını ve parolasını belirtmek için kullanmanız gerekir:

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

sürüm >= 1.0.45 pip bağımlılığı olarak azureml varsayılanları eklemeniz gerekir. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir. Ayrıca "en son" için çevre inferencing_stack_version özelliği ayarlamanız gerekir, bu web hizmeti tarafından gerekli belirli apt paketleri yükler. 

Ortamı tanımladıktan sonra, model [inferenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) nesnesi ile model ve web hizmetinin çalışacağı çıkarım ortamını tanımlayın.

```python
from azureml.core.model import InferenceConfig
# Use environment in InferenceConfig
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
```

Bu noktada, dağıtıma devam edebilirsiniz. Örneğin, aşağıdaki kod parçacığı çıkarım yapılandırmasını ve özel görüntüyü kullanarak bir web hizmetini yerel olarak dağıtacak:

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Dağıtım hakkında daha fazla bilgi için [Azure Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

Python ortamınızı özelleştirme hakkında daha fazla bilgi için [eğitim ve dağıtım ortamları oluşturma ve yönetme'ye](how-to-use-environments.md)bakın. 

### <a name="use-an-image-with-the-machine-learning-cli"></a>Machine Learning CLI ile görüntü kullanma

> [!IMPORTANT]
> Şu anda Machine Learning CLI, çalışma alanınız veya herkese açık depolarınız için Azure Konteyner Kayıt Defteri'ndeki görüntüleri kullanabilir. Bağımsız özel kayıtlardaki görüntüleri kullanamaz.

Machine Learning CLI'yi kullanarak bir model dağıtmadan önce, özel görüntüyü kullanan bir [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) oluşturun. Ardından, ortama başvuran bir çıkarım yapılandırma dosyası oluşturun. Ayrıca, çıkarım yapılandırma dosyasında ortamı doğrudan tanımlayabilirsiniz. Aşağıdaki JSON belgesi, genel konteyner kayıt defterindeki bir görüntüye nasıl başvurulsüreceğini gösterir. Bu örnekte, ortam satır içinde tanımlanır:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
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

Bu dosya komutu `az ml model deploy` ile kullanılır. Parametre `--ic` çıkarım yapılandırma dosyasını belirtmek için kullanılır.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

ML CLI'yi kullanarak bir modeli dağıtma hakkında daha fazla bilgi için Azure Machine Learning makalesi [için CLI uzantısının](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) "model kaydı, profil oluşturma ve dağıtım" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Nerede ve nasıl dağıtılanınacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.
* [Azure Pipelines'ı kullanarak makine öğrenimi modellerini nasıl eğitip dağıtacaklarını](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops)öğrenin.
