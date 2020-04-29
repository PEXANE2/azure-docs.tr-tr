---
title: Bing Resim Arama API'si uç noktaları
titleSuffix: Azure Cognitive Services
description: Resim Arama API 'SI üç uç nokta içerir. Endpoint 1, Web 'den görüntüleri döndürür. Uç nokta 2 ımageınsights döndürüyor. Uç nokta 3, popüler resimleri döndürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 38416f6a580d270aefc287de0c198bd418a44db9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74072639"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Bing Resim Arama API'si uç noktaları

**Resim arama API 'si** üç uç nokta içerir.  Endpoint 1, bir sorguya göre Web 'den görüntüleri döndürür. Uç nokta 2 [ımageınsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse)döndürüyor.  Uç nokta 3, popüler resimleri döndürür.

## <a name="endpoints"></a>Uç Noktalar

Bing API kullanarak görüntü sonuçlarını almak için aşağıdaki uç noktalardan birine bir istek gönderin. Daha fazla belirtim tanımlamak için üstbilgiler ve URL parametreleri kullanın.

**Uç nokta 1:** Kullanıcı tarafından `?q=""`tanımlanan arama sorgusuyla ilgili görüntüleri döndürür.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Uç nokta 2:** Ya `GET` `POST`da kullanarak bir görüntüyle ilgili öngörüleri döndürür.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
GET isteği, görüntüyü içeren Web sayfaları gibi bir görüntüyle ilgili öngörüleri döndürür. Bir `GET` Istek ile [ınsi, Stoken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) parametresini ekleyin.

Ya da bir `POST` isteğin gövdesine bir ikili görüntü ekleyebilir ve [modüller](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini olarak `RecognizedEntities`ayarlayabilirsiniz. Bu işlem, görüntüdeki kişiler hakkında bilgi döndüren sonraki `GET` bir istekte parametre olarak kullanılacak bir [ınsibir sToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken) döndürür.  Kullanılarak `modules` başka `All` bir çağrı yapmadan sonuçları `RecognizedEntities` `POST` hariç tüm öngörüleri almak için olarak ayarlayın `insightsToken`.


**Uç nokta 3:** Başkaları tarafından yapılan arama isteklerine göre popüler olan görüntüleri döndürür. Görüntüler, örneğin, önemli kişilere veya olaylara bağlı olarak farklı kategorilere ayrılmıştır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Popüler resimleri destekleyen pazarların bir listesi için bkz. [popüler görüntüler](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar vb. hakkında ayrıntılar için [Bing resim arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) başvurusuna bakın.
## <a name="response-json"></a>JSON yanıtı
Bir görüntü arama isteğine yönelik yanıt, sonuçları JSON nesneleri olarak içerir. Sonuçları ayrıştırma örnekleri için bkz. [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API 'leri türlerine göre sonuçlar döndüren arama eylemlerini destekler.Tüm arama uç noktaları sonuçları JSON yanıt nesneleri olarak döndürür. Tüm uç noktalar, Boylam, enlem ve arama yarıçapı tarafından belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her uç nokta tarafından desteklenen parametreler hakkında tüm bilgiler için, her tür için başvuru sayfalarına bakın.
Görüntü arama API 'sini kullanan temel isteklerin örnekleri için bkz. [resim arama Hızlı Başlat](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).
