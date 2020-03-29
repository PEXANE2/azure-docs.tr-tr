---
title: Yanıtları görüntülemek için sıralamayı kullanma - Bing Entity Search
titleSuffix: Azure Cognitive Services
description: Bing Entity Search API'nin döndürdüğü yanıtları gününde sıralamayı nasıl kullanır
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 110cef117683b20170649a231226c8193496edf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68423906"
---
# <a name="using-ranking-to-display-entity-search-results"></a>Varlık arama sonuçlarını görüntülemek için sıralamayı kullanma  

Her varlık arama yanıtı, Bing Entity Search API tarafından döndürülen arama sonuçlarını nasıl görüntülemeniz gerektiğini belirten bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) yanıtı içerir. Sıralama yanıt grupları kutup, ana çizgi ve kenar çubuğu içeriğine sonuç verir. Kutup sonucu en önemli veya belirgin sonucudur ve ilk görüntülenmelidir. Kalan sonuçları geleneksel bir ana çizgi ve kenar çubuğu biçiminde görüntülemezseniz, ana hat içeriğinin kenar çubuğu içeriğinden daha yüksek görünürlük sağlamanız gerekir. 
  
Her grup [içinde, Öğeler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) dizisi içeriğin görünmesi gereken sırayı tanımlar. Her öğe, bir yanıt içinde sonucu tanımlamak için iki yol sağlar.  
 

|Alan | Açıklama  |
|---------|---------|
|`answerType` ve `resultIndex` | `answerType`yanıtı (Varlık veya Yer) `resultIndex` tanımlar ve bu yanıtiçinde bir sonucu (örneğin, bir varlık) tanımlar. Endeks 0'dan başlar.|
|`value`    | `value`Yanıt içinde bir yanıtın veya sonucun kimliğiyle eşleşen bir kimlik içerir. Yanıt veya sonuçlar kimliği içerir, ancak her ikisini birden içermez. |
  
Kullanarak `answerType` ve `resultIndex` iki adımlı bir süreçtir. İlk olarak, görüntülenecek sonuçları içeren yanıtı tanımlamak için kullanın. `answerType` Ardından, `resultIndex` sonucu görüntülemek için bu yanıtın sonuçlarına dizin oluşturmayı kullanın. (Değer `answerType` [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) nesnesindeki alanın adıdır.) Tüm yanıt sonuçlarını birlikte görüntülemeniz gerekiyorsa, sıralama yanıt öğesi `resultIndex` alanı içermez.

Kimliği kullanmak, sıralama kimliğini bir yanıtın kimliğiyle veya sonuçlarından biriyle eşleştirmenizi gerektirir. Yanıt nesnesi `id` bir alan içeriyorsa, tüm yanıt sonuçlarını birlikte görüntüleyin. Örneğin, nesne `Entities` alanı içeriyorsa, `id` tüm varlık makalelerini birlikte görüntüleyin. `Entities` Nesne alanı içermiyorsa, `id` her varlık `id` bir alan içerir ve sıralama yanıtı varlıkları Yerler sonuçlarıyla karıştırır.  
  
## <a name="ranking-response-example"></a>Sıralama yanıtı örneği

Aşağıdaki bir örnek [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse)gösterir.
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Bu sıralama yanıtına göre, kenar çubuğu Jimi Hendrix ile ilgili iki varlık sonuçlarını görüntüleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-entities-search-single-page-app.md)
