---
title: Arama sonuçlarında sayfalama - Bing Arama API'leri
titleSuffix: Azure Cognitive Services
description: Bing Arama API'lerinden arama sonuçlarını nasıl sayfalayan öğrenebilirsiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: aahi
ms.openlocfilehash: ea883bb294a8769b3c9be1e0eafc2e3e7c811b48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73481731"
---
# <a name="how-to-page-through-results-from-the-bing-search-apis"></a>Bing Arama API'lerinden elde edilen sonuçlar alanasıl sayfalanır?

Bing Web, Özel, Resim, Haber veya Video Arama API'lerine çağrı gönderdiğinde, Bing sorguyla alakalı olabilecek toplam sonuç sayısının bir alt kümesini döndürür. Tahmini toplam kullanılabilir sonuç sayısını elde etmek için yanıt `totalEstimatedMatches` nesnesinin alanına erişin. 

Örnek: 

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

## <a name="paging-through-search-results"></a>Arama sonuçlarında gezinme

Kullanılabilir sonuçları sayfalamak için, `count` `offset` isteğinizi gönderirken ve sorgu parametrelerini kullanın.  

> [!NOTE]
>
> * Bing Video, Resim ve Haber API'leri ile sayfalama`/video/search`yalnızca`/news/search`genel video`/image/search`( ), haberler ( ) ve görüntü ( ) aramaları için geçerlidir. Popüler konular ve kategoriler arasında gezinme desteklenmez.  
> * Alan, `TotalEstimatedMatches` geçerli sorgunun toplam arama sonuçları nın tahminidir. Parametreleri ve `count` `offset` parametreleri ayarladığınızda, bu tahmin değişebilir.

| Parametre | Açıklama                                                                                                                                                                |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `count`   | Yanıtta döndürülecek sonuç sayısını belirtir. `count`Varsayılan değerinin ve isteyebileceğiniz maksimum sonuç sayısının API'ye göre değiştiğini unutmayın. Bu değerleri [Sonraki adımlar](#next-steps)altında başvuru belgelerinde bulabilirsiniz. |
| `offset`  | Atlayacak sonuç sayısını belirtir. Sıfır `offset` tabanlıdır ve ()`totalEstimatedMatches` - `count`den daha az olmalıdır.                                           |

Örnek olarak, sayfa başına 15 sonuç görüntülemek istiyorsanız, `count` sonuçların ilk `offset` sayfasını almak için 15 ve 0 olarak ayarlarsınız. Sonraki her API çağrısı için, `offset` 15 oranında artış olur. Aşağıdaki örnekte, ofset 45'ten başlayarak 15 web sayfası isteyiş verilmiştir.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Varsayılan `count` değeri kullanıyorsanız, yalnızca API `offset` çağrılarınızda sorgu parametresini belirtmeniz gerekir.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Bing Resim ve Video API'lerini kullanırken, yinelenen arama sonuçlarından kaçınmak için `nextOffset` bu değeri kullanabilirsiniz. Veya `Images` `Videos` yanıt nesnelerinden değeri alın ve `offset` parametre ile isteklerinizde kullanın.  

> [!NOTE]
> Bing Web Arama API'sı, web sayfaları, resimler, videolar ve haberler içerebilen arama sonuçlarını döndürür. Bing Web Arama API'sının arama sonuçlarına sayfa araettiğinizde, resim veya haber gibi diğer yanıt türlerini değil, yalnızca [Web Sayfalarını](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webpage)sayfalarsınız. Nesnelerdeki `WebPage` arama sonuçları, diğer yanıt türlerinde de görünen sonuçlar içerebilir.
>
> Sorgu parametresini herhangi bir `responseFilter` filtre değeri belirtmeden kullanıyorsanız, bu parametreyi ve `count` `offset` parametreleri kullanmayın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Web Arama API'leri nedir?](bing-api-comparison.md)
* [Bing Web Araması API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)
* [Bing Özel Arama API v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
* [Bing Haber Arama API v7 referans](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)
* [Bing Video Arama API v7 başvuru](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference)
* [Bing Resim Arama API v7 başvuru](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
