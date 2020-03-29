---
title: Bing Haber Arama uç noktaları
titleSuffix: Azure Cognitive Services
description: Bu makalede, Haber arama API uç noktalarının bir özetini sağlar; haberler, en iyi haberler ve popüler haberler.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: dc7d16fe809e3e324f384b0d9e088dd7e6ab261c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74111490"
---
# <a name="bing-news-search-api-endpoints"></a>Bing Haber Arama API uç noktaları

Haber Arama API haber **makaleleri,** Web sayfaları, görüntüler, videolar ve [varlıklar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web)döndürür. Varlıklar bir kişi, yer veya konu hakkında özet bilgiler içerir.

## <a name="endpoints"></a>Uç Noktalar

Bing Haberler Arama API'sini kullanarak haber `GET` arama sonuçları almak için aşağıdaki uç noktalardan birine bir istek gönderin. Üstbilgi ve URL parametreleri diğer belirtimleri tanımlar.

### <a name="news-items-by-search-query"></a>Arama sorgusuna göre haber öğeleri

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

Arama sorgusuna göre haber öğelerini döndürür. Arama sorgusu boşsa, API farklı kategorilerdeki en iyi haber makalelerini döndürecektir. Arama teriminizi kodlayan ve`q=""` parametreye ekleyerek url'ye göre bir sorgu gönderin. Kullanılabilirlik için [bkz.](language-support.md#supported-markets-for-news-search-endpoint)

### <a name="top-news-items-by-category"></a>Kategoriye göre en iyi haber öğeleri

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

En iyi haber öğelerini kategoriye göre döndürür. Özellikle üst iş, spor veya eğlence makaleleri kullanarak `category=business`talep edebilirsiniz , `category=sports`veya `category=entertainment`. `category` Parametre yalnızca `/news` URL ile kullanılabilir. Kategorileri belirtmek için bazı resmi gereksinimler vardır; sorgu `category` [parametresi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#query-parameters) belgelerine bakın. Arama teriminizi kodlayan ve`q=""` parametreye ekleyerek url'ye göre bir sorgu gönderin. Kullanılabilirlik için [bkz.](language-support.md#supported-markets-for-news-endpoint)

### <a name="trending-news-topics"></a>Popüler haber konuları 

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Şu anda sosyal ağlarda trend olan haber konularını döndürür. `/trendingtopics` Seçenek dahil edildiğinde, Bing araması gibi `freshness` diğer birkaç parametreyi yok sayar. `?q=""` Kullanılabilirlik için [bkz.](language-support.md#supported-markets-for-news-trending-endpoint)

## <a name="next-steps"></a>Sonraki adımlar

Üstbilgiler, parametreler, pazar kodları, yanıt nesneleri, hatalar, vb. hakkında ayrıntılı bilgi için [Bing Haberler arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) referansına bakın.

Her bitiş noktası tarafından desteklenen parametreler hakkında tam bilgi için her tür için başvuru sayfalarına bakın.
Haber arama API'sini kullanarak temel istekörnekleri için [Bing Haberler Arama Hızlı başlangıçlar](https://docs.microsoft.com/azure/cognitive-services/bing-news-search)bölümüne bakın.
