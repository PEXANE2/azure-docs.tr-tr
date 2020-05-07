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
ms.date: 03/06/2020
ms.openlocfilehash: 104e0892e2ad6bc6a0b3212722781f9498eee219
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744989"
---
# <a name="deploy-a-machine-learning-model-to-azure-functions-preview"></a>Azure Işlevlerine makine öğrenme modeli dağıtma (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Işlevlerinde bir işlev uygulaması olarak Azure Machine Learning bir modeli dağıtmayı öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hem de Azure Işlevleri genel kullanıma sunulduğunda, Işlevleri için Machine Learning hizmetinden bir modeli paketleyebilme özelliği önizlemededir.

Azure Machine Learning, eğitilen makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Artık Azure Machine Learning, bu makine öğrenimi modellerini [Azure işlevlerine dağıtılabilecek](https://docs.microsoft.com/azure/azure-functions/functions-deployment-technologies#docker-container)işlev uygulamalarına derlemek için Önizleme işlevselliğine sahiptir.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Çalışma alanınıza kayıtlı eğitilen makine öğrenimi modeli. Bir modeliniz yoksa, görüntü oluşturma öğreticisini kullanın: eğitim ve kayıt yapmak için [modeli eğitme](tutorial-train-models-with-aml.md) .

    > [!IMPORTANT]
    > Bu makaledeki kod parçacıkları aşağıdaki değişkenleri ayarlamış olduğunu varsayar:
    >
    > * `ws`-Azure Machine Learning çalışma alanınız.
    > * `model`-Dağıtılacak kayıtlı model.
    > * `inference_config`-Modelin çıkarım yapılandırması.
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
> Azure Işlevleri ile kullanmak üzere bir çıkarım yapılandırması oluştururken, bir [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py) nesnesi kullanmanız gerekir. Özel bir ortam tanımlıyorsanız, bir PIP bağımlılığı olarak >= 1.0.45 sürümü ile azureml-varsayılan değer eklemeniz gerektiğini unutmayın. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerir. Aşağıdaki örnek, bir ortam nesnesi oluşturmayı ve bunu bir çıkarım yapılandırmasıyla kullanmayı gösterir:
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
> Kod parçacığı, kayıtlı bir `model` model içerdiğini ve çıkarım ortamının yapılandırmasını `inference_config` içeren olduğunu varsayar. Daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

```python
from azureml.contrib.functions import package
from azureml.contrib.functions import BLOB_TRIGGER
blob = package(ws, [model], inference_config, functions_enabled=True, trigger=BLOB_TRIGGER, input_path="input/{blobname}.json", output_path="output/{blobname}_out.json")
blob.wait_for_creation(show_output=True)
# Display the package location/ACR path
print(blob.location)
```

Ne `show_output=True`zaman, Docker Build işleminin çıktısı gösterilir. İşlem tamamlandıktan sonra görüntü, çalışma alanınızın Azure Container Registry oluşturulur. Görüntü derlendikten sonra Azure Container Registry konum görüntülenir. Döndürülen konum biçimindedir `<acrinstance>.azurecr.io/package@sha256:<imagename>`.

> [!NOTE]
> İşlevlerin paketlenmesi Şu anda HTTP tetikleyicilerini, blob Tetikleyicileri ve Service Bus tetikleyicilerini desteklemektedir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Azure işlevleri bağlamaları](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob-trigger#blob-name-patterns).

> [!IMPORTANT]
> Görüntü dağıtımında kullanılan konum bilgilerini kaydedin.

## <a name="deploy-image-as-a-web-app"></a>Web uygulaması olarak görüntü dağıtma

1. Görüntüyü içeren Azure Container Registry oturum açma kimlik bilgilerini almak için aşağıdaki komutu kullanın. Daha `<myacr>` önce döndürülen değerle değiştirin `package.location`: 

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
    az appservice plan create --name myplanname --resource-group myresourcegroup --sku B1 --is-linux
    ```

    Bu örnekte, bir _Linux temel_ Fiyatlandırma Katmanı (`--sku B1`) kullanılır.

    > [!IMPORTANT]
    > Azure Machine Learning tarafından oluşturulan görüntüler Linux kullanır, bu nedenle `--is-linux` parametresini kullanmanız gerekir.

1. Web işi depolaması için kullanılacak depolama hesabını oluşturun ve bağlantı dizesini alın. Kullanmak `<webjobStorage>` istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name <webjobStorage> --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactive
    az storage account show-connection-string --resource-group myresourcegroup --name <webJobStorage> --query connectionString --output tsv
    ```

1. İşlev uygulaması oluşturmak için aşağıdaki komutu kullanın. Kullanmak `<app-name>` istediğiniz adla değiştirin. Ve `<acrinstance>` `<imagename>` değerlerini daha önce döndürülen `package.location` değerlerle değiştirin. Önceki `<webjobStorage>` adımda bulunan depolama hesabının adıyla değiştirin:

    ```azurecli-interactive
    az functionapp create --resource-group myresourcegroup --plan myplanname --name <app-name> --deployment-container-image-name <acrinstance>.azurecr.io/package:<imagename> --storage-account <webjobStorage>
    ```

    > [!IMPORTANT]
    > Bu noktada, işlev uygulaması oluşturulmuştur. Ancak, blob tetikleyicisi için bağlantı dizesini veya görüntüyü içeren Azure Container Registry kimlik bilgilerini sağlamadıysanız, işlev uygulaması etkin değil. Sonraki adımlarda, kapsayıcı kayıt defteri için bağlantı dizesini ve kimlik doğrulama bilgilerini sağlarsınız. 

1. Blob tetikleyici depolaması için kullanılacak depolama hesabını oluşturun ve bağlantı dizesini alın. Kullanmak `<triggerStorage>` istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name <triggerStorage> --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactiv
    az storage account show-connection-string --resource-group myresourcegroup --name <triggerStorage> --query connectionString --output tsv
    ```
    İşlev uygulamasına sağlamak için bu bağlantı dizesini kaydedin. Daha sonra, şunu sorduğumuz zaman kullanacağız`<triggerConnectionString>`

1. Depolama hesabındaki giriş ve çıkış için kapsayıcıları oluşturun. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az storage container create -n input --connection-string <triggerConnectionString>
    ```
    ```azurecli-interactive
    az storage container create -n output --connection-string <triggerConnectionString>
    ```

1. Tetikleyici bağlantı dizesini işlev uygulamasıyla ilişkilendirmek için aşağıdaki komutu kullanın. İşlev `<app-name>` uygulamasının adıyla değiştirin. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az functionapp config appsettings set --name <app-name> --resource-group myresourcegroup --settings "TriggerConnectionString=<triggerConnectionString>"
    ```
1. Aşağıdaki komutu kullanarak oluşturulan kapsayıcı ile ilişkili etiketi almanız gerekir. Daha `<username>` önce kapsayıcı kayıt defterinden döndürülen kullanıcı adıyla değiştirin:

    ```azurecli-interactive
    az acr repository show-tags --repository package --name <username> --output tsv
    ```
    Döndürülen değeri kaydedin, sonraki adımda `imagetag` ' de kullanılır.

1. İşlev uygulamasını kapsayıcı kayıt defterine erişmek için gereken kimlik bilgileriyle sağlamak için aşağıdaki komutu kullanın. İşlev `<app-name>` uygulamasının adıyla değiştirin. Ve `<acrinstance>` `<imagetag>` değerlerini, ÖNCEKI adımda bulunan az CLI çağrısındaki değerlerle değiştirin. Ve `<username>` `<password>` daha önce alınan ACR oturum açma bilgileriyle değiştirin:

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

## <a name="test-the-deployment"></a>Dağıtımı test etme

Görüntü yüklendikten ve uygulama kullanılabilir olduğunda, uygulamayı tetiklemek için aşağıdaki adımları kullanın:

1. Score.py dosyasının beklediği verileri içeren bir metin dosyası oluşturun. Aşağıdaki örnek, 10 sayıdan oluşan bir dizi bekleyen bir score.py ile çalışır:

    ```json
    {"data": [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10], [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]}
    ```

    > [!IMPORTANT]
    > Verilerin biçimi, score.py ve modelinizin beklediklerinize bağlıdır.

2. Bu dosyayı daha önce oluşturulan tetikleyici Depolama Blobu içindeki giriş kapsayıcısına yüklemek için aşağıdaki komutu kullanın. Verileri `<file>` içeren dosyanın adıyla değiştirin. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin. Bu örnekte, `input` daha önce oluşturulan giriş kapsayıcısının adıdır. Farklı bir ad kullandıysanız şu değeri değiştirin:

    ```azurecli-interactive
    az storage blob upload --container-name input --file <file> --name <file> --connection-string <triggerConnectionString>
    ```

    Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

    ```json
    {
    "etag": "\"0x8D7C21528E08844\"",
    "lastModified": "2020-03-06T21:27:23+00:00"
    }
    ```

3. İşlev tarafından üretilen çıktıyı görüntülemek için, oluşturulan çıkış dosyalarını listelemek için aşağıdaki komutu kullanın. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin. Bu örnekte, `output` daha önce oluşturulan çıkış kapsayıcısının adıdır. Farklı bir ad kullandıysanız şu değeri değiştirin::

    ```azurecli-interactive
    az storage blob list --container-name output --connection-string <triggerConnectionString> --query '[].name' --output tsv
    ```

    Bu komutun çıktısı öğesine `sample_input_out.json`benzerdir.

4. Dosyayı indirmek ve içerikleri incelemek için aşağıdaki komutu kullanın. Önceki `<file>` komutun döndürdüğü dosya adıyla değiştirin. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin: 

    ```azurecli-interactive
    az storage blob download --container-name output --file <file> --name <file> --connection-string <triggerConnectionString>
    ```

    Komut tamamlandıktan sonra dosyayı açın. Model tarafından döndürülen verileri içerir.

Blob tetikleyicilerini kullanma hakkında daha fazla bilgi için bkz. [Azure Blob depolama tarafından tetiklenen bir Işlev oluşturma](/azure/azure-functions/functions-create-storage-blob-triggered-function) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

* [İşlevler belgelerindeki](/azure/azure-functions/functions-create-function-linux-custom-image) işlevlerinizi uygulamanızı yapılandırma hakkında bilgi edinin.
* BLOB depolama, [Azure Blob depolama bağlamalarını](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob)tetikler hakkında daha fazla bilgi edinin.
* [Modelinizi Azure App Service Için dağıtın](how-to-deploy-app-service.md).
* [Web hizmeti olarak dağıtılan bir ML modelini kullanma](how-to-consume-web-service.md)
* [API Başvurusu](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py)
