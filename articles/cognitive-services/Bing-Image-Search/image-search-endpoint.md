---
title: Bing Resim Arama API'si uç noktaları
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si için kullanılabilir uç noktaların listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: fc1a0670767b134ad6e330fb41fc5564c754d91c
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883414"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Bing Resim Arama API'si uç noktaları

**Resim arama API 'si** üç uç nokta içerir.  Endpoint 1, bir sorguya göre Web 'den görüntüleri döndürür. Uç nokta 2 [ımageınsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse)döndürüyor.  Uç nokta 3, popüler resimleri döndürür.

## <a name="endpoints"></a>Uç Noktalar

Bing API kullanarak görüntü sonuçlarını almak için aşağıdaki uç noktalardan birine bir istek gönderin. Daha fazla belirtim tanımlamak için üstbilgiler ve URL parametreleri kullanın.

**Uç nokta 1:** Kullanıcı tarafından `?q=""`tanımlanan arama sorgusuyla ilgili görüntüleri döndürür.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Uç nokta 2:** `GET` Ya`POST`da kullanarak bir görüntüyle ilgili öngörüleri döndürür.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
GET isteği, görüntüyü içeren Web sayfaları gibi bir görüntüyle ilgili öngörüleri döndürür. Bir`GET` istek ile [ınsi, Stoken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) parametresini ekleyin.

Ya da bir `POST` isteğin gövdesine bir ikili görüntü ekleyebilir ve [modüller](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini olarak `RecognizedEntities`ayarlayabilirsiniz. Bu işlem, görüntüdeki [](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken) kişiler hakkında bilgi döndüren sonraki `GET` bir istekte parametre olarak kullanılacak bir ınsibir sToken döndürür.  Kullanılarak `modules` `All` başkabir`RecognizedEntities` çağrı yapmadan sonuçları`POST` hariç tüm öngörüleri almak için olarak ayarlayın. `insightsToken`


**Uç nokta 3:** Başkaları tarafından yapılan arama isteklerine göre popüler olan görüntüleri döndürür. Görüntüler, örneğin, önemli kişilere veya olaylara bağlı olarak farklı kategorilere ayrılmıştır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Popüler resimleri destekleyen pazarların bir listesi için bkz. [popüler görüntüler](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar vb. hakkında ayrıntılar için [Bing resim arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) başvurusuna bakın.
## <a name="response-json"></a>JSON yanıtı
Bir görüntü arama isteğine yönelik yanıt, sonuçları JSON nesneleri olarak içerir. Sonuçları ayrıştırma örnekleri için bkz. [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API 'leri türlerine göre sonuçlar döndüren arama eylemlerini destekler. Tüm arama uç noktaları sonuçları JSON yanıt nesneleri olarak döndürür.  Tüm uç noktalar, Boylam, enlem ve arama yarıçapı tarafından belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her uç nokta tarafından desteklenen parametreler hakkında tüm bilgiler için, her tür için başvuru sayfalarına bakın.
Görüntü arama API 'sini kullanan temel isteklerin örnekleri için bkz. [resim arama Hızlı Başlat](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).
