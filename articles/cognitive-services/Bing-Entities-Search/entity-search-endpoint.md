---
title: Bing Varlık Arama API'si uç noktası
titleSuffix: Azure Cognitive Services
description: Bing Varlık Arama API'si uç noktası ve gönderme istekleri hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 9d08091d0ea6869d13e294e60454f85a84f672ad
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68424045"
---
# <a name="bing-entity-search-api-endpoint"></a>Bing Varlık Arama API'si uç noktası


Bing Varlık Arama API'si bir sorguya göre Web 'den varlıkları döndüren bir uç nokta vardır. Bu arama sonuçları JSON içinde döndürülür.

## <a name="get-entity-results-from-the-endpoint"></a>Uç noktadan varlık sonuçları al

**Bing API**'yi kullanarak varlık sonuçları almak için aşağıdaki uç noktaya `GET` bir istek gönderin. Arama isteğinizi özelleştirmek için [üst bilgileri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) ve [sorgu parametrelerini](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) kullanın. Arama istekleri `?q=` parametresi kullanılarak gönderilebilir.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Varlık Arama API'si nedir?](overview.md)

## <a name="see-also"></a>Ayrıca bkz. 

Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar ve daha fazlası hakkında daha fazla bilgi için [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) Reference makalesine bakın.
