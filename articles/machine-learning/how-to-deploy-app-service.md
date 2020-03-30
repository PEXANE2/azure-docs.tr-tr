---
title: ML modellerini Azure Uygulama Hizmetine dağıtın (önizleme)
titleSuffix: Azure Machine Learning
description: Azure Uygulama Hizmeti'ndeki bir Web Uygulamasına bir model dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/27/2019
ms.openlocfilehash: 3e6cfde20d9f4d56af836e06b0c9a84010dea47b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282826"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Azure Uygulama Hizmetine bir makine öğrenme modeli dağıtma (önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Uygulama Hizmeti'nde web uygulaması olarak Azure Machine Learning'den bir modeli nasıl dağıtılayın öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hem de Azure Uygulama Hizmeti genel olarak kullanılabilir olsa da, Machine Learning hizmetinden Uygulama Hizmeti'ne bir model dağıtma olanağı önizlemededir.

Azure Machine Learning ile eğitimli makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Bu resim, verileri alan, modele gönderen ve sonra yanıtı döndüren bir web hizmeti içerir. Azure Uygulama Hizmeti görüntüyü dağıtmak için kullanılabilir ve aşağıdaki özellikleri sağlar:

* Gelişmiş güvenlik için gelişmiş [kimlik doğrulaması.](/azure/app-service/configure-authentication-provider-aad) Kimlik doğrulama yöntemleri hem Azure Etkin Dizini hem de çok faktörlü auth'u içerir.
* Yeniden dağıtmak zorunda kalmadan [otomatik ölçeklendirin.](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json)
* İstemciler ve hizmet arasında güvenli iletişim için [TLS desteği.](/azure/app-service/configure-ssl-certificate-in-code)

Azure Uygulama Hizmeti tarafından sağlanan özellikler hakkında daha fazla bilgi için [Uygulama Hizmetine genel bakış'a](/azure/app-service/overview)bakın.

> [!IMPORTANT]
> Dağıtılmış modelinizde kullanılan puanlama verilerini veya puanlama sonuçlarını günlüğe kaydetme yeteneğine ihtiyacınız varsa, bunun yerine Azure Kubernetes Hizmeti'ne dağıtmanız gerekir. Daha fazla bilgi için [bkz.](how-to-enable-data-collection.md)

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

    > [!IMPORTANT]
    > Azure Machine Learning SDK, web hizmetinin veri deponuza veya veri kümelerinize erişmesi için bir yol sağlamaz. Bir Azure Depolama hesabı gibi dağıtım dışında depolanan verilere erişmek için dağıtılan modele ihtiyacınız varsa, ilgili SDK'yı kullanarak özel bir kod çözümü geliştirmeniz gerekir. Örneğin, [Python için Azure Depolama SDK.](https://github.com/Azure/azure-storage-python)
    >
    > Senaryonuz için işe yada bir diğer alternatif de, puanlama sırasında veri depolarına erişim sağlayan [toplu iş tahminleridir.](how-to-use-parallel-run-step.md)

    Giriş komut dosyaları hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

* Giriş komut dosyasını veya modeli çalıştırmak için gereken yardımcı komut dosyaları veya Python/Conda paketleri gibi **bağımlılıklar**

Bu varlıklar bir __çıkarım yapılandırmasına__kapsüllenir. Çıkarım yapılandırması, giriş betiğine ve diğer bağımlılıklara başvurur.

> [!IMPORTANT]
> Azure Uygulama Hizmeti ile kullanılmak üzere çıkarım yapılandırması oluştururken bir [Ortam](https://docs.microsoft.com//python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py) nesnesi kullanmanız gerekir. Özel bir ortam tanımlıyorsanız, sürüm >= 1,0,45 pip bağımlılığı olarak azureml varsayılanları eklemeniz gerektiğini lütfen unutmayın. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir. Aşağıdaki örnek, bir ortam nesnesi oluşturmayı ve çıkarım yapılandırmasıyla kullanmayı gösterir:
>
> ```python
> from azureml.core.environment import Environment
> from azureml.core.conda_dependencies import CondaDependencies
> from azureml.core.model import InferenceConfig
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
> Azure Uygulama Hizmeti'ne dağıtılırken, bir __dağıtım yapılandırması__oluşturmanız gerekmez.

## <a name="create-the-image"></a>Görüntü oluşturma

Azure Uygulama Hizmetine dağıtılan Docker görüntüsünü oluşturmak için [Model.package'ı](https://docs.microsoft.com//python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config-none--generate-dockerfile-false-)kullanın. Aşağıdaki kod parçacığı, modelden yeni bir görüntü ve çıkarım yapılandırmasından nasıl yeni bir görüntü oluşturulabildiğini gösterir:

> [!NOTE]
> Kod snippet kayıtlı `model` bir model içeren ve `inference_config` çıkarım ortamı için yapılandırma içeren varsayar. Daha fazla bilgi için Azure [Machine Learning ile Modelleri Dağıt'a](how-to-deploy-and-where.md)bakın.

```python
from azureml.core import Model

package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
# Display the package location/ACR path
print(package.location)
```

Docker `show_output=True`yapı işleminin çıktısı gösterildiğinde. İşlem tamamlandığında, çalışma alanınız için görüntü Azure Kapsayıcı Kayıt Defteri'nde oluşturulur. Görüntü yüklendikten sonra Azure Konteyner Kayıt Defterinizdeki konum görüntülenir. Döndürülen konum biçimindedir. `<acrinstance>.azurecr.io/package:<imagename>` Örneğin, `myml08024f78fd10.azurecr.io/package:20190827151241`.

> [!IMPORTANT]
> Görüntüyü dağıtırken kullanıldığı gibi konum bilgilerini kaydedin.

## <a name="deploy-image-as-a-web-app"></a>Görüntüyü web uygulaması olarak dağıtma

1. Görüntüyü içeren Azure Kapsayıcı Kayıt Defteri'nin giriş kimlik bilgilerini almak için aşağıdaki komutu kullanın. Daha `<acrinstance>` önce döndürülen th `package.location`e değeri ile değiştirin:

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

    Bu örnekte, __temel__ fiyatlandırma`--sku B1`katmanı ( ) kullanılır.

    > [!IMPORTANT]
    > Azure Machine Learning tarafından oluşturulan görüntüler Linux'u kullanır, bu nedenle parametreyi `--is-linux` kullanmanız gerekir.

1. Web uygulamasını oluşturmak için aşağıdaki komutu kullanın. Kullanmak `<app-name>` istediğiniz adla değiştirin. Daha `<acrinstance>` `<imagename>` önce döndürülen `package.location` değerlerle değiştirin:

    ```azurecli-interactive
    az webapp create --resource-group myresourcegroup --plan myplanname --name <app-name> --deployment-container-image-name <acrinstance>.azurecr.io/package:<imagename>
    ```

    Bu komut, aşağıdaki JSON belgesine benzer bilgileri döndürür:

    ```json
    {
    "adminSiteName": null,
    "appServicePlanName": "myplanname",
    "geoRegion": "West Europe",
    "hostingEnvironmentProfile": null,
    "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myplanname",
    "kind": "linux",
    "location": "West Europe",
    "maximumNumberOfWorkers": 1,
    "name": "myplanname",
    < JSON data removed for brevity. >
    "targetWorkerSizeId": 0,
    "type": "Microsoft.Web/serverfarms",
    "workerTierName": null
    }
    ```

    > [!IMPORTANT]
    > Bu noktada, web uygulaması oluşturuldu. Ancak, görüntüyü içeren Azure Kapsayıcı Kayıt Defteri'ne kimlik bilgilerini sağlamadığınız için, web uygulaması etkin değildir. Bir sonraki adımda, kapsayıcı kayıt defteri için kimlik doğrulama bilgilerini sağlarsınız.

1. Web uygulamasına konteyner kayıt defterine erişmek için gereken kimlik bilgilerini sağlamak için aşağıdaki komutu kullanın. Kullanmak `<app-name>` istediğiniz adla değiştirin. Daha `<acrinstance>` `<imagename>` önce döndürülen `package.location` değerlerle değiştirin. Daha `<username>` `<password>` önce alınan ACR giriş bilgilerini değiştirin ve bu bilgilerle değiştirin:

    ```azurecli-interactive
    az webapp config container set --name <app-name> --resource-group myresourcegroup --docker-custom-image-name <acrinstance>.azurecr.io/package:<imagename> --docker-registry-server-url https://<acrinstance>.azurecr.io --docker-registry-server-user <username> --docker-registry-server-password <password>
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

Bu noktada, web uygulaması görüntüyü yüklemeye başlar.

> [!IMPORTANT]
> Görüntünün yüklenmesi birkaç dakika sürebilir. İlerlemeyi izlemek için aşağıdaki komutu kullanın:
>
> ```azurecli-interactive
> az webapp log tail --name <app-name> --resource-group myresourcegroup
> ```
>
> Görüntü yüklendikten ve site etkin hale geldikten sonra, `Container <container name> for site <app-name> initialized successfully and is ready to serve requests`günlük belirten bir ileti görüntüler.

Görüntü dağıtıldıktan sonra, aşağıdaki komutu kullanarak ana bilgisayar adını bulabilirsiniz:

```azurecli-interactive
az webapp show --name <app-name> --resource-group myresourcegroup
```

Bu komut, aşağıdaki ana bilgisayar `<app-name>.azurewebsites.net`adına benzer bilgileri döndürür - . Bu değeri hizmet için __temel url'nin__ bir parçası olarak kullanın.

## <a name="use-the-web-app"></a>Web Uygulamasını Kullanma

İstekleri modele aktaran web hizmeti `{baseurl}/score`. Örneğin, `https://<app-name>.azurewebsites.net/score`. Aşağıdaki Python kodu, URL'ye nasıl veri gönderilen ve yanıtı nasıl görüntülenebildiğini gösterir:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

## <a name="next-steps"></a>Sonraki adımlar

* Web Uygulamanızı Linux belgelerinde [Uygulama Hizmeti'nde](/azure/app-service/containers/) yapılandırmayı öğrenin.
* [Azure'da Otomatik Ölçekleme ile başlayın'da](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json)ölçeklendirme hakkında daha fazla bilgi edinin.
* [Azure Uygulama Hizmetinizde TLS/SSL sertifikası kullanın.](/azure/app-service/configure-ssl-certificate-in-code)
* [Uygulama Hizmeti uygulamanızı Azure Active Directory oturum unu kullanacak şekilde yapılandırın.](/azure/app-service/configure-authentication-provider-aad)
* [Web hizmeti olarak dağıtılan bir ML Modelini tüketin](how-to-consume-web-service.md)
