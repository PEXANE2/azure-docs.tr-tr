---
title: Ml modellerini Azure App Service dağıtma (Önizleme)
titleSuffix: Azure Machine Learning service
description: Azure App Service bir modeli bir Web uygulamasına dağıtmak için Azure Machine Learning hizmetini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/01/2019
ms.openlocfilehash: ada2a19de12c2f3f6b23fcc3d759afb0c747d37d
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897442"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Azure App Service bir makine öğrenimi modeli dağıtma (Önizleme)

Azure Machine Learning hizmetinden bir modeli Azure App Service Web uygulaması olarak nasıl dağıtacağınızı öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hizmeti hem de Azure App Service genel kullanıma sunulduğunda, Machine Learning hizmetinden App Service bir model dağıtma özelliği önizlemededir.

Azure Machine Learning hizmeti ile, eğitilen makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Bu görüntü, veri alan bir Web hizmeti içerir, bunu modele gönderir ve ardından yanıtı döndürür. Azure App Service görüntüyü dağıtmak için kullanılabilir ve aşağıdaki özellikleri sağlar:

* Gelişmiş güvenlik için Gelişmiş [kimlik doğrulaması](/azure/app-service/configure-authentication-provider-aad) . Kimlik doğrulama yöntemleri hem Azure Active Directory hem de Multi-Factor auth içerir.
* Yeniden dağıtmak zorunda kalmadan [Otomatik ölçeklendirme](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) .
* İstemcilerle hizmet arasında güvenli iletişim için [SSL desteği](/azure/app-service/app-service-web-ssl-cert-load) .

Azure App Service tarafından sunulan özellikler hakkında daha fazla bilgi için bkz. [App Service genel bakış](/azure/app-service/overview).

> [!IMPORTANT]
> Dağıtılan modelinizle kullanılan Puanlama verilerini veya Puanlama sonuçlarını günlüğe kaydetmek istiyorsanız, bunun yerine Azure Kubernetes hizmetine dağıtmanız gerekir. Daha fazla bilgi için bkz. [Üretim modellerinizde veri toplama](how-to-enable-data-collection.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.
* Çalışma alanınıza kayıtlı eğitilen makine öğrenimi modeli. Bir modeliniz yoksa, görüntü oluşturma öğreticisini kullanın: eğitim ve kayıt yapmak için [modeli eğitme](tutorial-train-models-with-aml.md) .

    > [!IMPORTANT]
    > Bu makaledeki kod parçacıkları aşağıdaki değişkenleri ayarlamış olduğunu varsayar:
    >
    > * `ws`-Azure Machine Learning çalışma alanınız.
    > * `model`-Dağıtılacak kayıtlı model.
    > * `inference_config`-Modelin çıkarım yapılandırması.
    >
    > Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="prepare-for-deployment"></a>Dağıtıma hazırlanma

Dağıtılmadan önce, modeli bir Web hizmeti olarak çalıştırmak için gerekenleri tanımlamanız gerekir. Aşağıdaki listede bir dağıtım için gereken temel öğeler açıklanmaktadır:

* Bir __giriş betiği__. Bu betik istekleri kabul eder, modeli kullanarak isteği puan eder ve sonuçları döndürür.

    > [!IMPORTANT]
    > Giriş betiği modelinize özeldir; gelen istek verilerinin biçimini, modelinizde beklenen verilerin biçimini ve istemcilere döndürülen verilerin biçimini anlamalıdır.
    >
    > İstek verileri modelinize uygun olmayan bir biçimdeyse, komut dosyası bunu kabul edilebilir bir biçime dönüştürebilir. Ayrıca, istemciye döndürmeden önce yanıtı dönüştürebilir.

    > [!IMPORTANT]
    > Azure Machine Learning SDK, Web hizmeti için veri deposuna veya veri kümelerine erişim için bir yol sağlamaz. Dağıtım dışında depolanan verilere (örneğin, bir Azure depolama hesabında) erişmek için dağıtılan modele ihtiyacınız varsa, ilgili SDK 'yı kullanarak özel bir kod çözümü geliştirmeniz gerekir. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python).
    >
    > Senaryonuza yönelik olabilecek başka bir alternatif de [toplu tahmindir](how-to-run-batch-predictions.md). Bu, Puanlama sırasında veri depolarına erişim sağlar.

    Giriş betikleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md).

* Giriş betiğini veya modelini çalıştırmak için gereken yardımcı betikler veya Python/Conda paketleri gibi **Bağımlılıklar**

Bu varlıklar bir __çıkarım yapılandırmasında__kapsüllenir. Çıkarım yapılandırması, giriş betiğine ve diğer bağımlılıklara başvurur.

> [!IMPORTANT]
> Azure App Service ile kullanım için bir çıkarım yapılandırması oluştururken, bir [ortam](https://docs.microsoft.com//python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py) nesnesi kullanmanız gerekir. Aşağıdaki örnek, bir ortam nesnesi oluşturmayı ve bunu bir çıkarım yapılandırmasıyla kullanmayı gösterir:
>
> ```python
> from azureml.core import Environment
> from azureml.core.environment import CondaDependencies
>
> # Create an environment and add conda dependencies to it
> myenv = Environment(name="myenv")
> # Enable Docker based environment
> myenv.docker.enabled = True
> # Build conda dependencies
> myenv.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
> ```

Ortamlar hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md).

Çıkarım yapılandırması hakkında daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md).

> [!IMPORTANT]
> Azure App Service ' ye dağıtım yaparken, bir __dağıtım yapılandırması__oluşturmanız gerekmez.

## <a name="create-the-image"></a>Görüntü oluşturma

Azure App Service dağıtılan Docker görüntüsünü oluşturmak için [model. Package](https://docs.microsoft.com//python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config--generate-dockerfile-false-)kullanın. Aşağıdaki kod parçacığı, modelden ve çıkarım yapılandırmasından nasıl yeni bir görüntü oluşturulacağını gösterir:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Ne `show_output=True`zaman, Docker Build işleminin çıktısı gösterilir. İşlem tamamlandıktan sonra görüntü, çalışma alanınızın Azure Container Registry oluşturulur.

## <a name="deploy-image-as-a-web-app"></a>Web uygulaması olarak görüntü dağıtma

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning çalışma alanınızı seçin. __Genel bakış__ bölümünde, çalışma alanının Azure Container Registry erişmek Için __kayıt defteri__ bağlantısını kullanın.

    [![Çalışma alanı için genel bakış ekran görüntüsü](media/how-to-deploy-app-service/workspace-overview.png)](media/how-to-deploy-app-service/workspace-overview-expanded.png)

2. Azure Container Registry, __depolar__' ı seçin ve ardından dağıtmak istediğiniz __görüntü adını__ seçin. Dağıtmak istediğiniz sürüm için __...__ girişini seçin ve ardından __Web uygulamasına dağıtın__.

    [![ACR 'den bir Web uygulamasına dağıtmanın ekran görüntüsü](media/how-to-deploy-app-service/deploy-to-web-app.png)](media/how-to-deploy-app-service/deploy-to-web-app-expanded.png)

3. Web uygulaması oluşturmak için bir site adı, abonelik, kaynak grubu sağlayın ve App Service planını/konumunu seçin. Son olarak __Oluştur__' u seçin.

    ![Yeni Web uygulaması iletişim kutusunun ekran görüntüsü](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Web uygulamasını kullanma

[Azure Portal](https://portal.azure.com), önceki adımda oluşturulan Web uygulamasını seçin. __Genel bakış__ bölümünden __URL 'yi__kopyalayın. Bu değer, hizmetin __temel URL 'sidir__ .

[![Web uygulaması için genel bakış ekran görüntüsü](media/how-to-deploy-app-service/web-app-overview.png)](media/how-to-deploy-app-service/web-app-overview-expanded.png)

İstekleri modele geçiren Web hizmeti konumunda `{baseurl}/score`bulunur. Örneğin: `https://mywebapp.azurewebsites.net/score`. Aşağıdaki Python kodu, URL 'ye veri göndermeyi ve yanıtı görüntülemeyi gösterir:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

print(headers)
    
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

* Web uygulamanızı yapılandırma hakkında daha fazla bilgi için [Linux 'ta App Service](/azure/app-service/containers/) bakın.
* Ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure 'Da otomatik ölçeklendirmeyi kullanmaya başlama](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json).
* SSL desteği hakkında daha fazla bilgi için bkz. [Azure App SERVICE SSL sertifikası kullanma](/azure/app-service/app-service-web-ssl-cert-load).
* Kimlik doğrulama hakkında daha fazla bilgi için bkz. [App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](/azure/app-service/configure-authentication-provider-aad).
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)