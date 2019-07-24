---
title: Kullanılabilir Web sayfaları aracılığıyla sayfa-Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama döndürebilirler tüm Web sayfalarının nasıl yapılacağını gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: 13b4cef624c636b8935897338badf3349f27c7f5
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405030"
---
# <a name="paging-webpages"></a>Sayfalama Web sayfaları 

Özel Arama API'si çağırdığınızda Bing, sonuçların bir listesini döndürür. Liste, sorguyla ilgili olabilecek Toplam sonuç sayısı alt kümesidir. Tahmini toplam kullanılabilir sonuç sayısını almak için, yanıt nesnesinin [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#totalestimatedmatches) alanına erişin.  
  
Aşağıdaki örnek, bir Web `totalEstimatedMatches` yanıtının içerdiği alanı gösterir.  
  
```  
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```  
  
Kullanılabilir Web sayfalarında sayfa eklemek için, [say](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#count) ve [kaydırma](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#offset) sorgu parametrelerini kullanın.  
  
`count` Parametresi, yanıtta döndürülecek sonuçların sayısını belirtir. Yanıtta talep alabileceğiniz en fazla sonuç sayısı 50 ' dir. Varsayılan değer 10 ' dur. Teslim edilen gerçek numara istenenden daha az olabilir.

`offset` Parametresi atlanacak sonuç sayısını belirtir. Sıfır tabanlıdır ve (`totalEstimatedMatches` - )değerindenküçükolmalıdır.`count` `offset`  
  
Sayfa başına 15 Web sayfası göstermek istiyorsanız sonuçların ilk sayfasını almak için 15 `count` ve `offset` 0 olarak ayarlanır. Sonraki her sayfa için 15 (örneğin, `offset` 15, 30) artırabilirsiniz.  
  
Aşağıda 45 uzaklığında başlayan 15 Web sayfası isteyen bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Uygulamanız için varsayılan `count` değer çalışırsa, yalnızca `offset` sorgu parametresini belirtmeniz gerekir.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Bu `TotalEstimatedAnswers` alan, geçerli sorgu için alabileceğiniz arama sonuçlarının toplam sayısının tahminidir.  Ve `count` `TotalEstimatedAnswers` parametrelerini ayarladığınızda, numara değişebilir. `offset` 

