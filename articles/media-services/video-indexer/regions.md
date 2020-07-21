---
title: Video Indexer kullanılabildiği bölgeler-Azure
titleSuffix: Azure Media Services
description: Bu makalede Azure Media Services Video Indexer kullanılabildiği Azure bölgeleri ele sunulmuştur.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: kumud
ms.openlocfilehash: 45da0556d074fdb81002f73cd89d0c35cb37276e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86530058"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Video Indexer var olan Azure bölgeleri

Video Indexer API 'Leri, çağrının yönlendirileceği Azure bölgesine ayarlamanız gereken bir **konum** parametresi içerir. Bu [, video Indexer kullanılabildiği bir Azure bölgesi](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)olmalıdır.

## <a name="locations"></a>Konumlar

`location`Parametreye, değeri olarak Azure bölge kodu adı verilmelidir. Önizleme modunda Video Indexer kullanıyorsanız, `"trial"` değeri olarak koymanız gerekir. `trial`, parametresi için varsayılan değerdir `location` . Aksi takdirde, hesabınızın bulunduğu Azure bölgesinin kod adını almak ve çağrın ' a yönlendirilmek için, [Azure CLI](/cli/azure)'de aşağıdaki satırı çalıştırabilirsiniz:

```azurecli-interactive
az account list-locations
```

Yukarıda gösterilen satırı çalıştırdığınızda tüm Azure bölgelerinin bir listesini alırsınız. Aradığınız *DisplayName* 'e sahip Azure bölgesine gidin ve **konum** parametresi için *ad* değerini kullanın.

Örneğin, Azure bölgesi Batı ABD 2 için (aşağıda görüntülenir), **konum** parametresi için "westus2" kullanacaksınız.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Sonraki adımlar

- [API 'Leri kullanarak dil modelini özelleştirme](customize-language-model-with-api.md)
- [API 'Leri kullanarak markalar modelini özelleştirme](customize-brands-model-with-api.md)
- [API 'Leri kullanarak kişi modelini özelleştirme](customize-person-model-with-api.md)
