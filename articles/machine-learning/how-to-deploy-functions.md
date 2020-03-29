---
title: ML modellerini Azure İşlevleri Uygulamalarına dağıtma (önizleme)
titleSuffix: Azure Machine Learning
description: Bir modeli Azure İşlevleri Uygulamasına dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: vaidyas
ms.reviewer: larryfr
ms.date: 03/06/2020
ms.openlocfilehash: d03a3d482d147d3bc69354ee09dfe0b187610a09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927433"
---
# <a name="deploy-a-machine-learning-model-to-azure-functions-preview"></a>Azure İşlevlerine makine öğrenimi modeli dağıtma (önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure İşlevler'de işlev uygulaması olarak Azure Machine Learning'den bir modeli nasıl dağıtılayın öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hem de Azure İşlevleri genel olarak kullanılabilir olsa da, İşlevler için Machine Learning hizmetinden bir modeli paketleme olanağı önizlemededir.

Azure Machine Learning ile eğitimli makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Azure Machine Learning artık bu makine öğrenimi modellerini [Azure İşlevlerine](https://docs.microsoft.com/azure/azure-functions/functions-deployment-technologies#docker-container)dağıtılabilen işlev uygulamalarına dönüştürmek için önizleme işlevine sahiptir.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [çalışma alanı](how-to-manage-workspace.md) oluştur makalesine bakın.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Çalışma alanınızda kayıtlı eğitimli bir makine öğrenme modeli. Bir modeliniz yoksa, Resim [sınıflandırma öğreticisini kullanın:](tutorial-train-models-with-aml.md) bir modeli eğitmek ve kaydetmek için model eğitin.

    > [!IMPORTANT]
    > Bu makaledeki kod parçacıkları aşağıdaki değişkenleri ayarladığınızvarsa varsaymaktadır:
    >
    > * `ws`- Azure Makine Öğrenimi çalışma alanınız.
    > * `model`- Dağıtılacak kayıtlı model.
    > * `inference_config`- Model için çıkarım yapılandırması.
    >
    > Bu değişkenleri ayarlama hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma

Dağıtmadan önce, modeli bir web hizmeti olarak çalıştırmak için gerekenleri tanımlamanız gerekir. Aşağıdaki liste, dağıtım için gereken temel öğeleri açıklar:

* Bir __giriş komut dosyası__. Bu komut dosyası istekleri kabul eder, modeli kullanarak isteği puanlar ve sonuçları döndürür.

    > [!IMPORTANT]
    > Giriş komut dosyası modelinize özgüdür; gelen istek verilerinin biçimini, modeliniz tarafından beklenen verilerin biçimini ve istemcilere döndürülen verilerin biçimini anlamanız gerekir.
    >
    > İstek verileri modeliniz tarafından kullanılabilir olmayan bir biçimdeyse, komut dosyası bunu kabul edilebilir bir biçime dönüştürebilir. Ayrıca istemciye dönmeden önce yanıtı dönüştürebilir.
    >
    > Varsayılan olarak işlevler için paketleme yaparken, giriş metin olarak kabul edilir. Girişin ham baytlarını tüketmek istiyorsanız (örneğin Blob tetikleyicileri için), [ham verileri kabul etmek için AMLRequest'i](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where#binary-data)kullanmalısınız.


* Giriş komut dosyasını veya modeli çalıştırmak için gereken yardımcı komut dosyaları veya Python/Conda paketleri gibi **bağımlılıklar**

Bu varlıklar bir __çıkarım yapılandırmasına__kapsüllenir. Çıkarım yapılandırması, giriş betiğine ve diğer bağımlılıklara başvurur.

> [!IMPORTANT]
> Azure İşlevleri ile kullanılmak üzere çıkarım yapılandırması oluştururken bir [Çevre](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py) nesnesi kullanmanız gerekir. Özel bir ortam tanımlıyorsanız, sürüm >= 1,0,45 pip bağımlılığı olarak azureml varsayılanları eklemeniz gerektiğini lütfen unutmayın. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir. Aşağıdaki örnek, bir ortam nesnesi oluşturmayı ve çıkarım yapılandırmasıyla kullanmayı gösterir:
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

Ortamlar hakkında daha fazla bilgi için [bkz.](how-to-use-environments.md)

Çıkarım yapılandırması hakkında daha fazla bilgi için [Azure Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

> [!IMPORTANT]
> İşlevlere dağıtılırken, __dağıtım yapılandırması__oluşturmanız gerekmez.

## <a name="install-the-sdk-preview-package-for-functions-support"></a>Fonksiyon desteği için SDK önizleme paketini yükleyin

Azure İşlevler için paket oluşturmak için SDK önizleme paketini yüklemeniz gerekir.

```bash
pip install azureml-contrib-functions
```

## <a name="create-the-image"></a>Görüntü oluşturma

Azure İşlevler'e dağıtılan Docker görüntüsünü oluşturmak için [azureml.contrib.functions.package'ı](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py) veya kullanmak istediğiniz tetikleyici için belirli paket işlevini kullanın. Aşağıdaki kod snippet modeli ve çıkarım yapılandırmabir blob tetikleyici ile yeni bir paket oluşturmak için nasıl gösterir:

> [!NOTE]
> Kod snippet kayıtlı `model` bir model içeren ve `inference_config` çıkarım ortamı için yapılandırma içeren varsayar. Daha fazla bilgi için Azure [Machine Learning ile Modelleri Dağıt'a](how-to-deploy-and-where.md)bakın.

```python
from azureml.contrib.functions import package
from azureml.contrib.functions import BLOB_TRIGGER
blob = package(ws, [model], inference_config, functions_enabled=True, trigger=BLOB_TRIGGER, input_path="input/{blobname}.json", output_path="output/{blobname}_out.json")
blob.wait_for_creation(show_output=True)
# Display the package location/ACR path
print(blob.location)
```

Docker `show_output=True`yapı işleminin çıktısı gösterildiğinde. İşlem tamamlandığında, çalışma alanınız için görüntü Azure Kapsayıcı Kayıt Defteri'nde oluşturulur. Görüntü yüklendikten sonra Azure Konteyner Kayıt Defterinizdeki konum görüntülenir. Döndürülen konum biçimindedir. `<acrinstance>.azurecr.io/package@sha256:<hash>`

> [!NOTE]
> Şu anda http tetikleyicileri, Blob tetikleyicileri ve Servis veri veri metotları tetikleyicileri işlevleri için paketleme destekler. Tetikleyiciler hakkında daha fazla bilgi için [Azure İşlevleri bağlamalarına](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob-trigger#blob-name-patterns)bakın.

> [!IMPORTANT]
> Görüntüyü dağıtırken kullanıldığı gibi konum bilgilerini kaydedin.

## <a name="deploy-image-as-a-web-app"></a>Görüntüyü web uygulaması olarak dağıtma

1. Görüntüyü içeren Azure Kapsayıcı Kayıt Defteri'nin giriş kimlik bilgilerini almak için aşağıdaki komutu kullanın. Daha `<myacr>` önce döndürülen değerle `package.location`değiştir: 

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

1. Hizmeti dağıtmak için zaten bir kaynak grubunuz veya uygulama hizmet planınız yoksa, aşağıdaki komutlar her ikisini birden nasıl oluşturabileceğinizi gösterir:

    ```azurecli-interactive
    az group create --name myresourcegroup --location "West Europe"
    az appservice plan create --name myplanname --resource-group myresourcegroup --sku B1 --is-linux
    ```

    Bu örnekte, bir _Linux_ `--sku B1`temel fiyatlandırma katmanı ( ) kullanılır.

    > [!IMPORTANT]
    > Azure Machine Learning tarafından oluşturulan görüntüler Linux'u kullanır, bu nedenle parametreyi `--is-linux` kullanmanız gerekir.

1. Web iş depolama için kullanmak ve bağlantı dizesini almak için depolama hesabı oluşturun. Kullanmak `<webjobStorage>` istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name <webjobStorage> --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactive
    az storage account show-connection-string --resource-group myresourcegroup --name <webJobStorage> --query connectionString --output tsv
    ```

1. İşlev uygulamasını oluşturmak için aşağıdaki komutu kullanın. Kullanmak `<app-name>` istediğiniz adla değiştirin. Daha `<acrinstance>` `<imagename>` önce döndürülen `package.location` değerlerle değiştirin. Önceki `<webjobStorage>` adımdaki depolama hesabının adı ile değiştirin:

    ```azurecli-interactive
    az functionapp create --resource-group myresourcegroup --plan myplanname --name <app-name> --deployment-container-image-name <acrinstance>.azurecr.io/package:<imagename> --storage-account <webjobStorage>
    ```

    > [!IMPORTANT]
    > Bu noktada, işlev uygulaması oluşturuldu. Ancak, görüntüiçeren Azure Kapsayıcı Kayıt Defteri'ne blob tetikleyicisi veya kimlik bilgileri için bağlantı dizesi sağlamadığınıziçin, işlev uygulaması etkin değildir. Sonraki adımlarda, bağlantı dizesini ve kapsayıcı kayıt defteri için kimlik doğrulama bilgilerini sağlarsınız. 

1. Blob tetikleme depolaması için kullanılacak depolama hesabını oluşturun ve bağlantı dizesini alın. Kullanmak `<triggerStorage>` istediğiniz adla değiştirin.

    ```azurecli-interactive
    az storage account create --name <triggerStorage> --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactiv
    az storage account show-connection-string --resource-group myresourcegroup --name <triggerStorage> --query connectionString --output tsv
    ```
    İşlev uygulamasına sağlamak için bu bağlantı dizesini kaydedin. Biz daha sonra istediğimizde kullanacağız`<triggerConnectionString>`

1. Depolama hesabındaki giriş ve çıktı için kapsayıcıları oluşturun. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az storage container create -n input --connection-string <triggerConnectionString>
    ```
    ```azurecli-interactive
    az storage container create -n output --connection-string <triggerConnectionString>
    ```

1. Tetikleyici bağlantı dizesini işlev uygulamasıyla ilişkilendirmek için aşağıdaki komutu kullanın. İşlev uygulamasının adı ile değiştirin. `<app-name>` Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin:

    ```azurecli-interactive
    az functionapp config appsettings set --name <app-name> --resource-group myresourcegroup --settings "TriggerConnectionString=<triggerConnectionString>"
    ```
1. Aşağıdaki komutu kullanarak oluşturulan kapsayıcı ile ilişkili etiketi almak gerekir. Kapsayıcı `<username>` kayıt defterinden daha önce döndürülen kullanıcı adı ile değiştirin:

    ```azurecli-interactive
    az acr repository show-tags --repository package --name <username> --output tsv
    ```
    Döndürülen değeri kaydedin, bir `imagetag` sonraki adımda olarak kullanılacaktır.

1. İşlev uygulamasına kapsayıcı kayıt defterine erişmek için gereken kimlik bilgilerini sağlamak için aşağıdaki komutu kullanın. İşlev uygulamasının adı ile değiştirin. `<app-name>` Bir `<acrinstance>` `<imagetag>` önceki adımdaki AZ CLI çağrısının değerlerini değiştirin. Daha `<username>` `<password>` önce alınan ACR giriş bilgilerini değiştirin ve bu bilgilerle değiştirin:

    ```azurecli-interactive
    az functionapp config container set --name <app-name> --resource-group myresourcegroup --docker-custom-image-name <acrinstance>.azurecr.io/package:<imagetag> --docker-registry-server-url https://<acrinstance>.azurecr.io --docker-registry-server-user <username> --docker-registry-server-password <password>
    ```

    Bu komut, aşağıdaki JSON belgesine benzer bilgileri döndürür:

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
> Görüntünün yüklenmesi birkaç dakika sürebilir. Azure Portalı'nı kullanarak ilerlemeyi izleyebilirsiniz.

## <a name="test-the-deployment"></a>Dağıtımı test etme

Resim yüklendikten ve uygulama kullanılabilir hale geldiklerinde, uygulamayı tetiklemek için aşağıdaki adımları kullanın:

1. score.py dosyasının beklediği verileri içeren bir metin dosyası oluşturun. Aşağıdaki örnek, 10 sayıdan oluşan bir dizi bekleyen bir score.py çalışır:

    ```json
    {"data": [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10], [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]}
    ```

    > [!IMPORTANT]
    > Verilerin biçimi, score.py ve modelinizin ne beklediğine bağlıdır.

2. Bu dosyayı daha önce oluşturulan tetikleyici depolama blob'undaki giriş kapsayıcısına yüklemek için aşağıdaki komutu kullanın. Verileri `<file>` içeren dosyanın adı ile değiştirin. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin. Bu örnekte, `input` daha önce oluşturulan giriş kapsayıcısının adı verilmiştir. Farklı bir ad kullandıysanız, bu değeri değiştirin:

    ```azurecli-interactive
    az storage blob upload --container-name input --file <file> --name <file> --connection-string <triggerConnectionString>
    ```

    Bu komutun çıktısı aşağıdaki JSON'a benzer:

    ```json
    {
    "etag": "\"0x8D7C21528E08844\"",
    "lastModified": "2020-03-06T21:27:23+00:00"
    }
    ```

3. İşlev tarafından üretilen çıktıyı görüntülemek için, oluşturulan çıktı dosyalarını listelemek için aşağıdaki komutu kullanın. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin. Bu örnekte, `output` daha önce oluşturulan çıkış kapsayıcısının adıdır. Farklı bir ad kullandıysanız, bu değeri değiştirin:

    ```azurecli-interactive
    az storage blob list --container-name output --connection-string <triggerConnectionString> --query '[].name' --output tsv
    ```

    Bu komutun çıktısı `sample_input_out.json`.

4. Dosyayı indirmek ve içeriğini incelemek için aşağıdaki komutu kullanın. Önceki `<file>` komuttarafından döndürülen dosya adı ile değiştirin. Daha `<triggerConnectionString>` önce döndürülen bağlantı dizesiyle değiştirin: 

    ```azurecli-interactive
    az storage blob download --container-name output --file <file> --name <file> --connection-string <triggerConnectionString>
    ```

    Komut tamamlandıktan sonra dosyayı açın. Model tarafından döndürülen verileri içerir.

Blob tetikleyicilerini kullanma hakkında daha fazla bilgi için Azure Blob depolama makalesi [tarafından tetiklenen bir işlev oluştur'a](/azure/azure-functions/functions-create-storage-blob-triggered-function) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Fonksiyonlar](/azure/azure-functions/functions-create-function-linux-custom-image) dokümanında İşlevler Uygulamanızı yapılandırmayı öğrenin.
* Blob depolama tetikleyicileri [Azure Blob depolama ciltleri](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob)hakkında daha fazla bilgi edinin.
* [Modelinizi Azure Uygulama Hizmeti'ne dağıtın.](how-to-deploy-app-service.md)
* [Web hizmeti olarak dağıtılan bir ML Modelini tüketin](how-to-consume-web-service.md)
* [API Başvurusu](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py)
