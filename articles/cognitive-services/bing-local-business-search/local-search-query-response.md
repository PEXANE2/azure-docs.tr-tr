---
title: API sorguları ve yanıtları gönderme ve kullanma - Bing Yerel İş Arama
titleSuffix: Azure Cognitive Services
description: Bing Yerel İşletme Arama API'si ile arama sorgularının nasıl gönderilip kullanılacağını öğrenmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: rosh
ms.openlocfilehash: 25bcdb89002fec4f9b67b091996d7bf80bcf21c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74326735"
---
# <a name="sending-and-using-bing-local-business-search-api-queries-and-responses"></a>Bing Yerel İşletme Arama API sorgularını ve yanıtlarını gönderme ve kullanma

Bing Yerel İşletme Arama API'sinden, bir arama sorgusunu bitiş noktasına göndererek ve gerekli üstbilgiyi `Ocp-Apim-Subscription-Key` de ekleyerek yerel sonuçlar alabilirsiniz. Kullanılabilir [üstbilgi](local-search-reference.md#headers) ve [parametrelerin](local-search-reference.md#query-parameters)yanı sıra, Aramalar aranacak alan için [coğrafi sınırlar](specify-geographic-search.md) ve döndürülecek yerlerin [kategorileri](local-search-query-response.md) belirterek özelleştirilebilir.

## <a name="creating-a-request"></a>İstek oluşturma

Bing Yerel İş Arama API'sine bir istek göndermek için, bir arama terimini API bitiş noktasına eklemeden önce `q=` ve `Ocp-Apim-Subscription-Key` üstbilgi de dahil olmak üzere parametreye ekleyerek ekleyin. Örnek:

`https://api.cognitive.microsoft.com/bing/localbusinesses/v7.0/search?q=restaurant+in+Bellevue`

Tam istek URL sözdizimi aşağıda gösterilmiştir. Bing Yerel İş Arama [API'si hızlı başlangıçlar](quickstarts/local-quickstart.md)ve istek gönderme hakkında daha fazla bilgi için [üstbilgiler](local-search-reference.md#headers) ve [parametreler](local-search-reference.md#query-parameters) için başvuru içeriğine bakın. 

Yerel arama kategorileri hakkında bilgi için [Bing Yerel İş Arama API'si için Arama kategorilerini](local-categories.md)görün.

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search[?q][&localCategories][&cc][&mkt][&safesearch][&setlang][&count][&first][&localCircularView][&localMapView]
```

## <a name="using-responses"></a>Yanıtları kullanma

Bing Yerel İş Arama API'sindeki JSON yanıtları bir `SearchResponse` nesne içerir. `places` API, alandaki ilgili arama sonuçlarını döndürecektir. sonuç bulunmazsa, `places` alan yanıta dahil edilmez.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

```
{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": "restaurant in Bellevue"
   },
   "places": {
      "totalEstimatedMatches": 10,
. . . 
```

### <a name="search-result-attributes"></a>Arama sonucu öznitelikleri

API tarafından döndürülen JSON sonuçları aşağıdaki öznitelikleri içerir:

* _type
* adres
* entityPresentationInfo
* coğrafi
* id
* ad
* routeablePoint
* Telefon
* url

Üstbilgiler, parametreler, pazar kodları, yanıt nesneleri, hatalar, vb. hakkında genel bilgi için [Bing Yerel Arama API v7](local-search-reference.md) referansına bakın.

> [!NOTE]
> Siz veya sizin adınıza üçüncü bir taraf, Microsoft'a ait olmayan herhangi bir hizmeti veya özelliği sınama, geliştirmek, eğitmek, dağıtmak veya kullanıma sunmak amacıyla Yerel Arama API'sinden herhangi bir veriyi kullanamaz, saklayabilir, saklayabilir, depolayabilir, paylaşamaz veya dağıtamaz. 


## <a name="example-json-response"></a>Örnek JSON yanıtı

Aşağıdaki JSON yanıtı sorgu `?q=restaurant+in+Bellevue`tarafından belirtilen arama sonuçlarını içerir.

```json
Vary: Accept-Encoding
BingAPIs-TraceId: 5376FFEB65294E24BB9F91AD70545826
BingAPIs-SessionId: 06ED7CEC80F746AA892EDAAC97CB0CB4
X-MSEdge-ClientID: 112C391E72C0624204153594738C63DE
X-MSAPI-UserState: aeab
BingAPIs-Market: en-US
X-Search-ResponseInfo: InternalResponseTime=659,MSDatacenter=CO4
X-MSEdge-Ref: Ref A: 5376FFEB65294E24BB9F91AD70545826 Ref B: BY3EDGE0306 Ref C: 2018-10-16T16:26:15Z
apim-request-id: fe54f585-7c54-4bf5-8b92-b9bede2b710a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Cache-Control: max-age=0, private
Date: Tue, 16 Oct 2018 16:26:15 GMT
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Content-Length: 978
Content-Type: application/json; charset=utf-8
Expires: Tue, 16 Oct 2018 16:25:15 GMT

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "restaurant Bellevue"
  },
  "places": {
    "totalEstimatedMatches": 50,
    "value": [{
      "_type": "LocalBusiness",
      "id": "https:\/\/cognitivegblppe.azure-api.net\/api\/v7\/#Places.0",
      "name": "Facing East Taiwanese Restaurant",
      "url": "http:\/\/litadesign.wix.com\/facingeastrestaurant",
      "entityPresentationInfo": {
        "entityScenario": "ListItem",
        "entityTypeHints": ["Place", "LocalBusiness", "Restaurant"]
      },
      "geo": {
        "latitude": 47.6199188232422,
        "longitude": -122.202796936035
      },
      "routablePoint": {
        "latitude": 47.6199188232422,
        "longitude": -122.201713562012
      },
      "address": {
        "streetAddress": "1075 Bellevue Way NE Ste B2",
        "addressLocality": "Bellevue",
        "addressRegion": "WA",
        "postalCode": "98004",
        "addressCountry": "US",
        "neighborhood": "Bellevue",
        "text": "1075 Bellevue Way NE Ste B2, Bellevue, WA 98004"
      },
      "telephone": "(425) 688-2986"
    }],
    "searchAction": {
      "location": [{
        "name": "Bellevue, Washington"
      }],
      "query": "restaurant"
    }
  }
}
 
```

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="next-steps"></a>Sonraki adımlar
- [Yerel İş Arama sıcağınızda](quickstarts/local-quickstart.md)
- [Yerel İş Arama Java quickstart](quickstarts/local-search-java-quickstart.md)
- [Yerel İş Arama Düğümü hızlı başlat](quickstarts/local-search-node-quickstart.md)
- [Yerel İş Arama Python quickstart](quickstarts/local-search-python-quickstart.md)
