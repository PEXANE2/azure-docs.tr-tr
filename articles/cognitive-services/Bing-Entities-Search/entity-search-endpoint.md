---
title: Bing Varlık Arama API bitiş noktası
titleSuffix: Azure Cognitive Services
description: Bing Entity Search API'de, sorguyu temel alan varlıkları Web'den döndüren bir uç nokta vardır. Bu arama sonuçları JSON'da döndürülür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: e7f5af42d0bb0079746cc9e64b621adfebd565d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072665"
---
# <a name="bing-entity-search-api-endpoint"></a>Bing Varlık Arama API bitiş noktası


Bing Entity Search API'de, sorguyu temel alan varlıkları Web'den döndüren bir uç nokta vardır. Bu arama sonuçları JSON'da döndürülür.

## <a name="get-entity-results-from-the-endpoint"></a>Bitiş noktasından varlık sonuçları alma

**Bing API'sını**kullanarak varlık sonuçları `GET` almak için aşağıdaki bitiş noktasına bir istek gönderin. Arama isteğinizi özelleştirmek için [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) ve [sorgu parametrelerini](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) kullanın. Arama istekleri `?q=` parametre kullanılarak gönderilebilir.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Varlık Arama API'si nedir?](overview.md)

## <a name="see-also"></a>Ayrıca bkz. 

Üstbilgiler, parametreler, pazar kodları, yanıt nesneleri, hatalar ve daha fazlası hakkında daha fazla bilgi için [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) başvuru makalesine bakın.
