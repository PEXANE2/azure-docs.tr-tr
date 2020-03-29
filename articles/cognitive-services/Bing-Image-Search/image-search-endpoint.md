---
title: Bing Resim Arama API'si için uç noktalar
titleSuffix: Azure Cognitive Services
description: Resim Arama API'si üç uç nokta içerir. Endpoint 1 görüntüleri web'den döndürür. Endpoint 2, ImageInsights'ı döndürür. Endpoint 3, popüler görüntüleri döndürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 38416f6a580d270aefc287de0c198bd418a44db9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072639"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Bing Resim Arama API'si için uç noktalar

**Resim Arama API'si** üç uç nokta içerir.  Endpoint 1, bir sorguya dayalı olarak Web'den görüntüleri döndürür. Endpoint [2, ImageInsights'ı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse)döndürür.  Endpoint 3, popüler görüntüleri döndürür.

## <a name="endpoints"></a>Uç Noktalar

Bing API'sini kullanarak görüntü sonuçları almak için aşağıdaki uç noktalardan birine bir istek gönderin. Diğer belirtimleri tanımlamak için üstbilgi ve URL parametrelerini kullanın.

**Bitiş Noktası 1:** Kullanıcıtarafından tanımlanan arama sorgusuyla alakalı görüntüleri `?q=""`verir.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Bitiş Noktası 2:** Bir görüntü yle ilgili öngörüleri, aşağıdakilerden birini `GET` kullanarak veya `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
GET isteği, görüntüyü içeren Web sayfaları gibi bir resimle ilgili öngörüleri döndürür. Bir `GET` istekle [öngörülerToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) parametresini ekleyin.

Veya, bir isteğin gövdesine ikili `POST` bir görüntü ekleyebilir ve `RecognizedEntities` [modülparametresini](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) . Bu, görüntüdeki kişiler hakkında bilgi döndüren sonraki `GET` bir istekte parametre olarak kullanılacak bir [öngörüToken'i](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken) döndürür.  Kullanarak `modules` `All` başka bir arama yapmadan `RecognizedEntities` sonuçları `POST` dışında, tüm anlayışlar `insightsToken`almak için ayarlayın.


**Bitiş Noktası 3:** Başkaları tarafından yapılan arama isteklerine göre eğilim gösteren görüntüleri döndürür. Görüntüler, örneğin kayda değer kişilere veya olaylara bağlı olarak farklı kategorilere ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Popüler görüntüleri destekleyen pazarların listesi için [Bkz. Trending Images.](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)

Üstbilgiler, parametreler, piyasa kodları, yanıt nesneleri, hatalar, vb. hakkında ayrıntılı bilgi için [Bing Image Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) referansına bakın.
## <a name="response-json"></a>Yanıt JSON
Görüntü arama isteğine verilen yanıt, JSON nesneleri olarak sonuçları içerir. Sonuçları ayrıştırma örnekleri için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) ve [kaynak koduna](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source)bakın.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'leri, sonuçları türüne göre döndüren arama eylemlerini destekler.Tüm arama bitiş noktaları JSON yanıt nesneleri olarak sonuçları döndürer. Tüm uç noktalar, boylam, enlem ve arama yarıçapına göre belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her bitiş noktası tarafından desteklenen parametreler hakkında tam bilgi için her tür için başvuru sayfalarına bakın.
Resim arama API'sini kullanarak temel istek örnekleri için [Bkz. Resim Arama Hızlı başlar.](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web)
