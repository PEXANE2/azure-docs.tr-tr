---
title: Azure Işlevleri uygulamalarına ml modellerini dağıtma (Önizleme)
titleSuffix: Azure Machine Learning
description: Azure Işlevleri uygulamasına model dağıtmak için Azure Machine Learning kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: vaidyas
ms.reviewer: larryfr
ms.date: 11/22/2019
ms.openlocfilehash: 321f985bd375e6fa4337e060bb15d318ea306ab4
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77116743"
---
# <a name="deploy-a-machine-learning-model-to-azure-functions-preview"></a>Azure Işlevlerine makine öğrenme modeli dağıtma (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Işlevlerinde bir işlev uygulaması olarak Azure Machine Learning bir modeli dağıtmayı öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hem de Azure Işlevleri genel kullanıma sunulduğunda, Işlevleri için Machine Learning hizmetinden bir modeli paketleyebilme özelliği önizlemededir.

Azure Machine Learning, eğitilen makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Artık Azure Machine Learning, bu makine öğrenimi modellerini [Azure işlevlerine dağıtılabilecek](https://docs.microsoft.com/azure/azure-functions/functions-deployment-technologies#docker-container)işlev uygulamalarına derlemek için Önizleme işlevselliğine sahiptir.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Çalışma alanınıza kayıtlı eğitilen makine öğrenimi modeli. Bir modeliniz yoksa, görüntü oluşturma öğreticisini kullanın: eğitim ve kayıt yapmak için [modeli eğitme](tutorial-train-models-with-aml.md) .

    > [!IMPORTANT]
    > Bu makaledeki kod parçacıkları aşağıdaki değişkenleri ayarlamış olduğunu varsayar:
    >
    > * `ws`-Azure Machine Learning çalışma alanınız.
    > * `model`-dağıtılacak kayıtlı model.
    > * `inference_config`-modelin çıkarım yapılandırması.
    >
    > Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma

Dağıtılmadan önce, modeli bir Web hizmeti olarak çalıştırmak için gerekenleri tanımlamanız gerekir. Aşağıdaki listede bir dağıtım için gereken temel öğeler açıklanmaktadır:

* Bir __giriş betiği__. Bu betik istekleri kabul eder, modeli kullanarak isteği puan eder ve sonuçları döndürür.

    > [!IMPORTANT]
    > Giriş betiği modelinize özeldir; gelen istek verilerinin biçimini, modelinizde beklenen verilerin biçimini ve istemcilere döndürülen verilerin biçimini anlamalıdır.
    >
    > İstek verileri modelinize uygun olmayan bir biçimdeyse, komut dosyası bunu kabul edilebilir bir biçime dönüştürebilir. Ayrıca, istemciye döndürmeden önce yanıtı dönüştürebilir.
    >
    > Varsayılan olarak, işlevler için paketleme sırasında giriş metin olarak değerlendirilir. Girişin ham baytlarını kullanmak istiyorsanız (örneğin, blob Tetikleyicileri için), [ham verileri kabul etmek Için Amlrequest](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where#binary-data)' i kullanmanız gerekir.


* Giriş betiğini veya modelini çalıştırmak için gereken yardımcı betikler veya Python/Conda paketleri gibi **Bağımlılıklar**

Bu varlıklar bir __çıkarım yapılandırmasında__kapsüllenir. Çıkarım yapılandırması, giriş betiğine ve diğer bağımlılıklara başvurur.

> [!IMPORTANT]
> Azure Işlevleri ile kullanmak üzere bir çıkarım yapılandırması oluştururken, bir [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py) nesnesi kullanmanız gerekir. Özel bir ortam tanımlıyorsanız, bir PIP bağımlılığı olarak > = 1.0.45 sürümü ile azureml-varsayılan değer eklemeniz gerektiğini unutmayın. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerir. Aşağıdaki örnek, bir ortam nesnesi oluşturmayı ve bunu bir çıkarım yapılandırmasıyla kullanmayı gösterir:
>
> ```python
> from azureml.core.environment import Environment
> from azureml.core.conda_dependencies import CondaDependencies
>
> # Create an environment and add conda dependencies to it
> myenv = Environment(name="myenv")
> # Enable Docker based environment
> myenv.docker.enabled = True
> # Build conda dependencies
> myenv.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'],
>                                                            pip_packages=['azureml-defaults'])
> inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
> ```

Ortamlar hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md).

Çıkarım yapılandırması hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

> [!IMPORTANT]
> IŞLEVLERE dağıtım yaparken, bir __dağıtım yapılandırması__oluşturmanız gerekmez.

## <a name="install-the-sdk-preview-package-for-functions-support"></a>İşlevler desteği için SDK önizleme paketini yükler

Azure Işlevlerine yönelik paketler oluşturmak için SDK önizleme paketini yüklemelisiniz.

```bash
pip install azureml-contrib-functions
```

## <a name="create-the-image"></a>Görüntü oluşturma

Azure Işlevlerine dağıtılan Docker görüntüsünü oluşturmak için, kullanarak ilgilendiğiniz tetikleyici için [azureml. contrib. Functions. Package](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py) veya belirli paket işlevini kullanın. Aşağıdaki kod parçacığı, model ve çıkarım yapılandırmasından bir blob tetikleyicisi ile nasıl yeni bir paket oluşturulacağını göstermektedir:

> [!NOTE]
> Kod parçacığı, `model` kayıtlı bir model içerdiğini ve `inference_config`, çıkarım ortamının yapılandırmasını içerdiğini varsayar. Daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

```python
from azureml.contrib.functions import package
from azureml.contrib.functions import BLOB_TRIGGER
blob = package(ws, [model], inference_config, functions_enabled=True, trigger=BLOB_TRIGGER, input_path="input/{blobname}.json", output_path="output/{blobname}_out.json")
blob.wait_for_creation(show_output=True)
# Display the package location/ACR path
print(blob.location)
```

`show_output=True`, Docker Build işleminin çıktısı gösterilir. İşlem tamamlandıktan sonra görüntü, çalışma alanınızın Azure Container Registry oluşturulur. Görüntü derlendikten sonra Azure Container Registry konum görüntülenir. Döndürülen konum `<acrinstance>.azurecr.io/package@sha256:<hash>`biçimindedir.

> [!NOTE]
> İşlevlerin paketlenmesi Şu anda HTTP tetikleyicilerini, blob Tetikleyicileri ve Service Bus tetikleyicilerini desteklemektedir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Azure işlevleri bağlamaları](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob?tabs=csharp#trigger---blob-name-patterns).

> [!IMPORTANT]
> Görüntü dağıtımında kullanılan konum bilgilerini kaydedin.

## <a name="deploy-image-as-a-web-app"></a>Web uygulaması olarak görüntü dağıtma

1. Görüntüyü içeren Azure Container Registry oturum açma kimlik bilgilerini almak için aşağıdaki komutu kullanın. `<myacr>`, daha önce `package.location`döndürülen değerle değiştirin: 

    ```azurecli-interactive
    az acr credential show --name <myacr>
    ```

    Bu komutun çıktısı aşağıdaki JSON belgesine benzer:

    ```json
    {
    "passwords": [
        {
        "name": "password",
        "value": "Iv0lRZQ9762LUJrFiffo3P4sWgk4q+nW"
        },
        {
        "name": "password2",
        "value": "=pKCxHatX96jeoYBWZLsPR6opszr==mg"
        }
    ],
    "username": "myml08024f78fd10"
    }
    ```

    __Kullanıcı adı__ ve __parolalardan__biri için değeri kaydedin.

1. Hizmeti dağıtmaya yönelik bir kaynak grubunuz veya App Service planınız yoksa, aşağıdaki komutlar her ikisinin de nasıl oluşturulacağını gösterir:

    ```azurecli-interactive
    az group create --name myresourcegroup --location "West Europe"
    az appservice plan create --name myplanname --resource-group myresourcegroup --sku EP1 --is-linux
    ```

    Bu örnekte, bir _Linux Premium_ fiyatlandırma katmanı (`--sku EP1`) kullanılır.

    > [!IMPORTANT]
    > Azure Machine Learning tarafından oluşturulan görüntüler Linux kullanır, bu nedenle `--is-linux` parametresini kullanmanız gerekir.

1. Web işi depolaması için kullanılacak depolama hesabını oluşturun ve bağlantı dizesini alın. `<webjobStorage>`, kullanmak istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name triggerStorage --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactive
    az storage account show-connection-string --resource-group myresourcegroup --name <webJobStorage> --query connectionString --output tsv
    ```

1. İşlev uygulaması oluşturmak için aşağıdaki komutu kullanın. `<app-name>`, kullanmak istediğiniz adla değiştirin. `<acrinstance>` ve `<imagename>`, daha önce döndürülen `package.location` değerlerle değiştirin. Değiştirme `<webjobStorage>`, önceki adımda bulunan depolama hesabının adıyla değiştirin:

    ```azurecli-interactive
    az functionapp create --resource-group myresourcegroup --plan myplanname --name <app-name> --deployment-container-image-name <acrinstance>.azurecr.io/package:<imagename> --storage-account <webjobStorage>
    ```

    > [!IMPORTANT]
    > Bu noktada, işlev uygulaması oluşturulmuştur. Ancak, blob tetikleyicisi için bağlantı dizesini veya görüntüyü içeren Azure Container Registry kimlik bilgilerini sağlamadıysanız, işlev uygulaması etkin değil. Sonraki adımlarda, kapsayıcı kayıt defteri için bağlantı dizesini ve kimlik doğrulama bilgilerini sağlarsınız. 

1. Blob tetikleyici depolaması için kullanılacak depolama hesabını oluşturun ve bağlantı dizesini alın. `<triggerStorage>`, kullanmak istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name <triggerStorage> --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactive
    az storage account show-connection-string --resource-group myresourcegroup --name <triggerStorage> --query connectionString --output tsv
    ```
    İşlev uygulamasına sağlamak için bu bağlantı dizesini kaydedin. Daha sonra `<triggerConnectionString>` isteytiğimiz zaman kullanacağız

1. Depolama hesabındaki giriş ve çıkış için kapsayıcıları oluşturun. `<triggerConnectionString>`, daha önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az storage container create -n input --connection-string <triggerConnectionString>
    ```
    ```azurecli-interactive
    az storage container create -n output --connection-string <triggerConnectionString>
    ```

1. Tetikleyici bağlantı dizesini işlev uygulamasıyla ilişkilendirmek için aşağıdaki komutu kullanın. `<app-name>`, işlev uygulamasının adıyla değiştirin. `<triggerConnectionString>`, daha önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az functionapp config appsettings set --name <app-name> --resource-group myresourcegroup --settings "TriggerConnectionString=<triggerConnectionString>"
    ```
1. Aşağıdaki komutu kullanarak oluşturulan kapsayıcı ile ilişkili etiketi almanız gerekir. `<username>`, daha önce kapsayıcı kayıt defterinden döndürülen kullanıcı adıyla değiştirin:

    ```azurecli-interactive
    az acr repository show-tags --repository package --name <username> --output tsv
    ```
    Döndürülen değeri kaydedin, bir sonraki adımda `imagetag` olarak kullanılacaktır.

1. İşlev uygulamasını kapsayıcı kayıt defterine erişmek için gereken kimlik bilgileriyle sağlamak için aşağıdaki komutu kullanın. `<app-name>`, kullanmak istediğiniz adla değiştirin. `<acrinstance>` ve `<imagetag>` değerini önceki adımdaki AZ CLı çağrısındaki değerlerle değiştirin. `<username>` ve `<password>` daha önce alınan ACR oturum açma bilgileriyle değiştirin:

    ```azurecli-interactive
    az functionapp config container set --name <app-name> --resource-group myresourcegroup --docker-custom-image-name <acrinstance>.azurecr.io/package:<imagetag> --docker-registry-server-url https://<acrinstance>.azurecr.io --docker-registry-server-user <username> --docker-registry-server-password <password>
    ```

    Bu komut aşağıdaki JSON belgesine benzer bilgiler döndürür:

    ```json
    [
    {
        "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
        "slotSetting": false,
        "value": "false"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_URL",
        "slotSetting": false,
        "value": "https://myml08024f78fd10.azurecr.io"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_USERNAME",
        "slotSetting": false,
        "value": "myml08024f78fd10"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
        "slotSetting": false,
        "value": null
    },
    {
        "name": "DOCKER_CUSTOM_IMAGE_NAME",
        "value": "DOCKER|myml08024f78fd10.azurecr.io/package:20190827195524"
    }
    ]
    ```

Bu noktada, işlev uygulaması görüntüyü yüklemeye başlar.

> [!IMPORTANT]
> Görüntünün yüklenmesi birkaç dakika sürebilir. Azure portalını kullanarak ilerlemeyi izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [İşlevler belgelerindeki](/azure/azure-functions/functions-create-function-linux-custom-image) işlevlerinizi uygulamanızı yapılandırma hakkında bilgi edinin.
* BLOB depolama, [Azure Blob depolama bağlamalarını](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob)tetikler hakkında daha fazla bilgi edinin.
* [Modelinizi Azure App Service Için dağıtın](how-to-deploy-app-service.md).
* [Web hizmeti olarak dağıtılan bir ML modelini kullanma](how-to-consume-web-service.md)
* [API Başvurusu](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py)
