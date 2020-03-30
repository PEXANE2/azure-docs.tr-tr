---
title: Video Dizinleyicinin kullanılabildiği bölgeler - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Azure Medya Hizmetleri Video Dizinleyici'nin kullanılabildiği Azure bölgeleri hakkında bahsedilir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: c91b38fcbfb9b517651adead010408425e519a82
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382758"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Video Dizinleyici'nin bulunduğu azure bölgeleri

Video Dizinleyici API'leri, çağrının yönlendirilmesi gereken Azure bölgesine ayarlamanız gereken bir **konum** parametresi içerir. Bu, [Video Dizinleyici'nin kullanılabildiği](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)bir Azure bölgesi olmalıdır.

## <a name="locations"></a>Konumlar

**Konum** parametresi değeri olarak Azure bölge kod adı verilmelidir. Video Indexer'ı önizleme modunda kullanıyorsanız, değer olarak *"deneme"* koymalısınız. Aksi takdirde, hesabınızın içinde bulunduğu ve aramanızın yönlendirilmesi gereken Azure bölgesinin kod adını almak için [Azure CLI'de](/cli/azure)aşağıdaki satırı çalıştırabilirsiniz:

```azurecli-interactive
az account list-locations
```

Yukarıda gösterilen satırı çalıştırdıktan sonra, tüm Azure bölgelerinin bir listesini alırsınız. Aradığınız *displayName'nin* bulunduğu Azure bölgesine gidin ve **konum** parametresi için *ad* değerini kullanın.

Örneğin, Azure bölgesi West US 2 (aşağıda gösterilmiştir) için **konum** parametresi için "westus2" kullanılır.

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

- [API'leri kullanarak Dil modelini özelleştir](customize-language-model-with-api.md)
- [API'leri kullanarak Markalar modelini özelleştirin](customize-brands-model-with-api.md)
- [API'leri kullanarak Kişi modelini özelleştir](customize-person-model-with-api.md)
