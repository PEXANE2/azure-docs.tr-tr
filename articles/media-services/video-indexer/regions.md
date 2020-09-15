---
title: Video Indexer kullanılabildiği bölgeler-Azure
titleSuffix: Azure Media Services
description: Bu makalede Azure Media Services Video Indexer kullanılabildiği Azure bölgeleri ele sunulmuştur.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/14/2020
ms.author: juliako
ms.openlocfilehash: 6ebdb22f50efbefc695f9752c6e6fc333571828c
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530955"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Video Indexer var olan Azure bölgeleri

Video Indexer API 'Leri, çağrının yönlendirileceği Azure bölgesine ayarlamanız gereken bir **konum** parametresi içerir. Bu [, video Indexer kullanılabildiği bir Azure bölgesi](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)olmalıdır.

## <a name="locations"></a>Konumlar

`location`Parametreye, değeri olarak Azure bölge kodu adı verilmelidir. Önizleme modunda Video Indexer kullanıyorsanız, `"trial"` değeri olarak koymanız gerekir. `trial` , parametresi için varsayılan değerdir `location` . Aksi takdirde, hesabınızın bulunduğu Azure bölgesinin kod adını almak ve çağrının yönlendirilmek zorunda olması için Azure portal kullanabilir veya bir [Azure CLI](/cli/azure) komutu çalıştırabilirsiniz.

### <a name="azure-portal"></a>Azure portalı

1. [Video Indexer](https://www.videoindexer.ai/) web sitesinde oturum açın.
1. Sayfanın sağ üst köşesinden **Kullanıcı hesapları** ' nı seçin.
1. Sağ üst köşedeki hesabınızın konumunu bulun.  

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/location/location1.png" alt-text="Konum":::
    
###  <a name="cli-command"></a>CLı komutu

```azurecli-interactive
az account list-locations
```

Yukarıda gösterilen satırı çalıştırdığınızda tüm Azure bölgelerinin bir listesini alırsınız. Aradığınız *DisplayName* 'e sahip Azure bölgesine gidin ve **konum** parametresi için *ad* değerini kullanın.

Örneğin, Azure bölgesi Batı ABD 2 için (aşağıda görüntülenir), **konum** parametresi için "westus2" kullanacaksınız.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/locations/westus2",
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
