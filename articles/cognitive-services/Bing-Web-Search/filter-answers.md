---
title: Arama sonuçları nasıl filtrelenebilen - Bing Web Arama API'sı
titleSuffix: Azure Cognitive Services
description: Bing'in yanıtta içerdiği yanıt türlerini (örneğin resimler, videolar ve haberler) 'yanıtFiltresi' sorgu parametresini kullanarak filtreleyebilirsiniz.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: scottwhi
ms.openlocfilehash: 6fa022f181e2061c6a7f3e08d1f2f501ddd9cac3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220275"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>Arama yanıtının içerdiği yanıtları filtreleme  

Web'i sorgularken, Bing arama için bulduğu tüm alakalı içeriği döndürür. Örneğin, arama sorgusu "yelken+dinghies" ise, yanıt aşağıdaki yanıtları içerebilir:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

## <a name="query-parameters"></a>Sorgu parametreleri

Bing tarafından döndürülen yanıtları filtrelemek için API'yi ararken aşağıdaki sorgu parametrelerini kullanın.  

### <a name="responsefilter"></a>YanıtFiltresi

Bing'in yanıtta içerdiği yanıt türlerini (örneğin resimler, videolar ve haberler) virgülle sınırlı bir yanıt listesi olan [yanıt Filtresi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#responsefilter) sorgu parametresini kullanarak filtreleyebilirsiniz. Bing bununla ilgili içerik bulursa yanıta bir yanıt eklenecektir. 

Görüntüler gibi yanıttan belirli yanıtları hariç `-` tutmak için, bir karakteri yanıt türüne hazırlayın. Örnek:

```
&responseFilter=-images,-videos
```

Aşağıda, resim, `responseFilter` video ve yelkenli dinghies haberleri ni istemek için nasıl kullanılacağı gösterilmektedir. Sorgu dizesini kodladiğinizde virgüller %2C olarak değişir.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Aşağıda, bir önceki sorgunun yanıtı gösterilmektedir. Bing alakalı video ve haber sonuçlarını bulamadığı için yanıt bunları içermez.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Bing önceki yanıtta video ve haber sonuçlarını döndürmese de, bu video ve haber içeriğinin olmadığı anlamına gelmez. Bu sadece sayfa onları içermediği anlamına gelir. Ancak, daha fazla sonuç [sayfa,](./paging-webpages.md) sonraki sayfalar da büyük olasılıkla bunları içerir. Ayrıca, [Video Arama API'sini](../bing-video-search/search-the-web.md) ve Haber Arama [API](../bing-news-search/search-the-web.md) uç noktalarını doğrudan ararsanız, yanıt büyük olasılıkla sonuçlar içerir.

Tek bir API'den sonuç almak için kullanmanız `responseFilter` önerilmez. Tek bir Bing API'sinden içerik istiyorsanız, doğrudan api'yi arayın. Örneğin, yalnızca resim almak için, Resim Arama API bitiş `https://api.cognitive.microsoft.com/bing/v7.0/images/search` noktasına veya diğer [Resimler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#endpoints) uç noktalarından birine bir istek gönderin. Tek API'yi çağırmak yalnızca performans nedenleriyle değil, içeriğe özgü API'ler daha zengin sonuçlar sunduğu için önemlidir. Örneğin, sonuçları filtrelemek için Web Arama API'sının kullanamayacağı filtrelerk kullanabilirsiniz.  

### <a name="site"></a>Site

Belirli bir etki alanından arama `site:` sonuçları almak için sorgu dizesi sorgu parametresini ekleyin.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE]
> Sorguya bağlı olarak, sorgu `site:` işlecikullanırsanız, yanıtın [güvenli Arama](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#safesearch) ayarından bağımsız olarak yetişkinlere uygun içerik içerme olasılığı vardır. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.

### <a name="freshness"></a>Güncellik

Web yanıt sonuçlarını Bing'in belirli bir dönemde keşfettiği web sayfalarıyla sınırlamak [için, tazelik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#freshness) sorgu parametresini aşağıdaki büyük/küçük harf duyarlı değerlerinden biriyle ayarlayın:

* `Day`— Bing'in son 24 saat içinde keşfettiği web sayfalarını döndür
* `Week`— Bing'in son 7 gün içinde keşfettiği web sayfalarını döndür
* `Month`— Son 30 gün içinde keşfedilen web sayfalarını döndür

Ayrıca bu parametreyi formda özel bir tarih `YYYY-MM-DD..YYYY-MM-DD`aralığına ayarlayabilirsiniz. 

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-01..2019-05-30`

Sonuçları tek bir tarihle sınırlamak için tazelik parametresini belirli bir tarihe ayarlayın:

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-04`

Bing'in filtre ölçütlerinize uyan web sayfası sayısı istediğiniz web sayfası sayısından (veya Bing'in döndürdettiği varsayılan sayıdan) daha azsa, sonuçlar belirtilen dönemin dışına çıkan web sayfalarını içerebilir.

## <a name="limiting-the-number-of-answers-in-the-response"></a>Yanıttaki yanıt sayısını sınırlama

Bing, JSON yanıtında birden çok yanıt türü döndürebilir. Örneğin, *yelken+dinghies*sorgularsanız, Bing `webpages` `images`, `videos`, `relatedSearches`, ve .

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Bing'in üst düzey iki yanıta (web sayfaları ve resimler) döndürdettiği yanıt sayısını sınırlamak için [yanıtSayısı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) sorgu parametresini 2 olarak ayarlayın.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yanıt yalnızca `webPages` ve `images`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

Sorgu parametresini `responseFilter` önceki sorguya ekleyip web sayfalarına ve haberlere ayarlarsanız, haberler sıralanmadığından yanıt yalnızca web sayfalarını içerir.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Sıralı olmayan yanıtları teşvik etme

Bing'in bir sorgu için döndüreceği en üst sırada yer alan yanıtlar web sayfaları, resimler, videolar ve ilgili Aramalar ise, yanıt bu yanıtları içerir. [YanıtSayısı'nı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) iki (2) olarak ayarlarsanız, Bing en iyi iki sırada yer alan yanıtı döndürür: web sayfaları ve resimler. Bing'in yanıta resim ve video eklemesini istiyorsanız, sorgu parametresini [tanıt'ı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#promote) belirtin ve bunu resimlere ve videolara ayarlayın.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yukarıdaki isteğe yanıt aşağıda veda edilebistir. Bing, en iyi iki yanıtı, web sayfalarını ve görüntüleri döndürür ve videoları yanıta dönüştürür.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Haberlere ayarlarsanız, `promote` yalnızca sıralanmış yanıtları tanıtabileceğiniz sıralı bir yanıt&mdash;olmadığından, yanıt haber yanıtını içermez.

Tanıtmak istediğiniz yanıtlar sınıra `answerCount` dahil değildir. Örneğin, sıralanan yanıtlar haber, resim ve videoysa ve `answerCount` siz `promote` 1 ve haberlere ayarlıyorsanız, yanıt haber ve görüntüler içerir. Veya, sıralanan yanıtlar videolar, resimler ve haberlerse, yanıt videolar ve haberler içerir.

Yalnızca sorgu `promote` parametresini `answerCount` belirtirseniz kullanabilirsiniz.
