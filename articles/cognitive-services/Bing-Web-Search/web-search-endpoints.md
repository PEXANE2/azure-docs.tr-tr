---
title: Web arama bitiş noktası
titleSuffix: Azure Cognitive Services
description: Web arama sonuçlarını almak `GET` için aşağıdaki bitiş noktasına bir istek gönderin. Üstbilgi ve URL parametreleri diğer belirtimleri tanımlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: aahi
ms.openlocfilehash: 2edc6a4cf4d4fb174c1b2067d8b3fb3b6f31859f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74111344"
---
# <a name="web-search-endpoint"></a>Web Arama bitiş noktası

**Web Arama API'sı** Web sayfalarını, haberleri, görüntüleri, videoları ve [varlıkları](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web)döndürür. Varlıklar, bir kişi, yer veya konu hakkında özet bilgilere sahiptir.

## <a name="endpoint"></a>Uç Nokta

Bing API'sini kullanarak Web arama `GET` sonuçları almak için aşağıdaki bitiş noktasına bir istek gönderin. Üstbilgi ve URL parametreleri diğer belirtimleri tanımlar.

**Uç Nokta**: Kullanıcının arama sorgusuyla `?q=""`ilgili web sonuçlarını verir.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Bitiş Noktası: Üstbilgiler, parametreler, pazar kodları, yanıt nesneleri, hatalar ve daha fazlası hakkında ayrıntılar için [Bing Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) başvurusuna bakın.

## <a name="response-json"></a>Yanıt JSON

Bir Web arama isteğine verilen yanıt, json nesneleri olarak tüm sonuçları içerir. Sonucu ayrıştma, her türdeki öğeleri işleyen yordamlar gerektirir. Örnekler için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) ve [kaynak koduna](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) bakın.

## <a name="next-steps"></a>Sonraki adımlar

**Bing** API'leri, sonuçları türüne göre döndüren arama eylemlerini destekler.Tüm arama bitiş noktaları JSON yanıt nesneleri olarak sonuçları döndürer. Tüm uç noktalar, boylam, enlem ve arama yarıçapına göre belirli bir dili ve konumu döndüren sorguları destekler.

Her bitiş noktası tarafından desteklenen parametreler hakkında tam bilgi için her tür için başvuru sayfalarına bakın.
Web arama API'sini kullanarak temel istekörnekleri için [bkz.](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web)
