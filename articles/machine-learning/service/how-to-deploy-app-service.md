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
ms.openlocfilehash: 84de9d53b19f5aa9b73570aa0d115d204e8b6596
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848209"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Azure App Service bir makine öğrenimi modeli dağıtma (Önizleme)

Azure Machine Learning hizmetinden bir modeli Azure App Service Web uygulaması olarak nasıl dağıtacağınızı öğrenin.

> [!IMPORTANT]
> Hem Azure Machine Learning hizmeti hem de Azure App Service genel kullanıma sunulduğunda, Machine Learning hizmetinden App Service bir model dağıtma özelliği önizlemededir.

Azure Machine Learning hizmeti ile, eğitilen makine öğrenimi modellerinden Docker görüntüleri oluşturabilirsiniz. Bu görüntü, veri alan bir Web hizmeti içerir, bunu modele gönderir ve ardından yanıtı döndürür. Azure App Service görüntüyü dağıtmak için kullanılabilir ve aşağıdaki özellikleri sağlar:

* İstemcilerle hizmet arasında güvenli iletişim için [SSL desteği](/azure/app-service/app-service-web-ssl-cert-load) .
* Yeniden dağıtmak zorunda kalmadan birden çok örneğe [ölçeklendirin](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) .
* Gelişmiş güvenlik için [Gelişmiş kimlik doğrulaması](/azure/app-service/configure-authentication-provider-aad) .

Azure App Service tarafından sunulan özellikler hakkında daha fazla bilgi için bkz. [App Service genel bakış](/azure/app-service/overview).

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.
* Çalışma alanınıza kayıtlı eğitilen makine öğrenimi modeli. Bir modeliniz yoksa, görüntü oluşturma öğreticisini kullanın: eğitim ve kayıt yapmak için [modeli eğitme](tutorial-train-models-with-aml.md) .
* Modelden oluşturulmuş bir Docker görüntüsü. Bir görüntünüz yoksa, bir görüntü oluşturmak için [görüntü sınıflandırması: modeli dağıt](tutorial-deploy-models-with-aml.md) ' ı kullanın.

## <a name="deploy-image-as-a-web-app"></a>Web uygulaması olarak görüntü dağıtma

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning çalışma alanınızı seçin. __Genel bakış__ bölümünde, çalışma alanının Azure Container Registry erişmek Için __kayıt defteri__ bağlantısını kullanın.

    ![Çalışma alanı için genel bakış ekran görüntüsü](media/how-to-deploy-app-service/workspace-overview.png)

2. Azure Container Registry, __depolar__' ı seçin ve ardından dağıtmak istediğiniz __görüntü adını__ seçin. Dağıtmak istediğiniz sürüm için __...__ girişini seçin ve ardından __Web uygulamasına dağıtın__.

    ![ACR 'den bir Web uygulamasına dağıtmanın ekran görüntüsü](media/how-to-deploy-app-service/deploy-to-web-app.png)

3. Web uygulaması oluşturmak için bir site adı, abonelik, kaynak grubu sağlayın ve App Service planını/konumunu seçin. Son olarak __Oluştur__' u seçin.

    ![Yeni Web uygulaması iletişim kutusunun ekran görüntüsü](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Web uygulamasını kullanma

[Azure Portal](https://portal.azure.com), önceki adımda oluşturulan Web uygulamasını seçin. __Genel bakış__ bölümünden __URL 'yi__kopyalayın. Bu değer, hizmetin __temel URL 'sidir__ .

![Web uygulaması için genel bakış ekran görüntüsü](media/how-to-deploy-app-service/web-app-overview.png)

İstekleri modele geçiren Web hizmeti konumunda `{baseurl}/score`bulunur. Örneğin: `https://mywebapp.azurewebsites.net/score`. Aşağıdaki Python kodu, URL 'ye veri göndermeyi ve yanıtı görüntülemeyi gösterir:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

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