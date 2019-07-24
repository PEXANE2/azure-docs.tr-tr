---
title: Bing Haber Arama sonuçları arasında sayfa oluşturma
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama API'si döndürdüğü haber makalelerini kullanarak nasıl sayfa oluşturacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: e5f8dce1a03e44758eea737ad2da419fa67c36a2
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423736"
---
# <a name="how-to-page-through-news-search-results"></a>Haber Arama sonuçları aracılığıyla sayfa oluşturma

Haber Arama API 'sini çağırdığınızda Bing, Sorgunuzla ilgili sonuçların bir listesini döndürür. Tahmini toplam kullanılabilir sonuç sayısını almak için, yanıt nesnesinin [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news-totalmatches) alanına erişin.  
  
Aşağıdaki örnek, bir haber `totalEstimatedMatches` yanıtının içerdiği alanı gösterir.  

```json
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Kullanılabilir makalelerle gezinmek için, [say](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#count) ve [kaydırma](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#offset) sorgu parametrelerini kullanın.  
 

|Parametre  |Açıklama  |
|---------|---------|
|`count`     | Yanıtta döndürülecek sonuçların sayısını belirtir. Yanıtta talep alabileceğiniz en fazla sonuç sayısı 100 ' dir. Varsayılan değer 10 ' dur. Teslim edilen gerçek numara istenenden daha az olabilir.        |
|`offset`     | Atlanacak sonuç sayısını belirtir. Sıfır tabanlıdır ve (`totalEstimatedMatches` - )değerindenküçükolmalıdır.`count` `offset`          |

Örneğin, sayfa başına 20 makale göstermek istiyorsanız sonuçların ilk sayfasını almak için 20 ve `count` `offset` 0 olarak ayarlanır. Sonraki her sayfa için 20 (örneğin, `offset` 20, 40) artırabilirsiniz.  
  
Aşağıdaki örnek 40 uzaklığında başlayan 20 haber makalelerini ister.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Uygulamanız için varsayılan `count` değer çalışırsa, aşağıdaki örnekte gösterildiği gibi `offset` yalnızca sorgu parametresini belirtin:  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Sayfalama, yalnızca haber arama (/News/Search) için geçerlidir ve konu başlıkları (/News/trendingkonular) veya haber kategorileri (/News) için geçerli değildir.

> [!NOTE]
> Bu `TotalEstimatedAnswers` alan, geçerli sorgu için alabileceğiniz arama sonuçlarının toplam sayısının tahminidir.  Ve `count` `TotalEstimatedAnswers` parametrelerini ayarladığınızda, numara değişebilir. `offset` 
