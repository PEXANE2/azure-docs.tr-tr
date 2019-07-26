---
title: Kullanılabilir videolar aracılığıyla sayfa oluşturma-Bing Video Arama
titleSuffix: Azure Cognitive Services
description: Bing Video Arama API'si tarafından döndürülen tüm videoların nasıl ekleneceğini öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: c3570d8772734595c6707ca8103006867a8eb47a
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68500714"
---
# <a name="paging-through-video-search-results"></a>Video arama sonuçlarıyla sayfalama

Bing Video Arama API'si, sorgunuz için bulduğu tüm arama sonuçlarının bir alt kümesini döndürür. API 'ye yapılan sonraki çağrılarla bu sonuçlar aracılığıyla disk belleğine alarak, bunları uygulamanızda alabilir ve gösterebilirsiniz.

> [!NOTE]
> Sayfalama yalnızca videolar araması (/videos/Search) için geçerlidir ve video öngörüleri (/videos/Details) veya popüler Videolar (/videos/trbitiriliyor) için geçerli değildir.

## <a name="total-estimated-matches"></a>Toplam tahmini eşleşme

Bulunan arama sonuçlarının tahmini sayısını almak için JSON yanıtında [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos-totalestimatedmatches) alanını kullanın.   
  
```json  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
## <a name="paging-through-videos"></a>Videolar aracılığıyla sayfalama

Kullanılabilir videolar aracılığıyla sayfa eklemek için isteğinizi gönderirken [say](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#count) ve [sapmayı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#offset) sorgulama parametrelerini kullanın.  
  

|Parametre  |Açıklama  |
|---------|---------|
|`count`     | Yanıtta döndürülecek sonuçların sayısını belirtir. Yanıtta talep alabileceğiniz en fazla sonuç sayısı 100 ' dir. Varsayılan değer 10 ' dur. Teslim edilen gerçek numara istenenden daha az olabilir.        |
|`offset`     | Atlanacak sonuç sayısını belirtir. Sıfır tabanlıdır ve (`totalEstimatedMatches` - )değerindenküçükolmalıdır.`count` `offset`          |

Örneğin, sayfa başına 20 makale göstermek istiyorsanız sonuçların ilk sayfasını almak için 20 ve `count` `offset` 0 olarak ayarlanır. Sonraki her sayfa için 20 (örneğin, `offset` 20, 40) artırabilirsiniz.  
  
Aşağıdaki örnek, 40 uzaklığında başlayarak 20 video ister.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

[Count](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#count)için varsayılan değeri kullanırsanız, aşağıdaki örnekte olduğu gibi yalnızca `offset` sorgu parametresini belirtmeniz gerekir.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Tek seferde 35 videolarla karşılaşırsanız, `offset` sorgu parametresini ilk talebinizdeki 0 olarak ayarlayın ve ardından sonraki her istekte 35 ile artırabilirsiniz. `offset` Ancak, bir sonraki yanıtın bazı sonuçları, önceki yanıttan yinelenen video sonuçları içerebilir. Örneğin, bir yanıttaki ilk iki video, önceki yanıtın son iki videosunda aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için, `Videos` nesnesinin [nextoffset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos-nextoffset) alanını kullanın.

Örneğin, bir kerede 30 video sayfa eklemek istiyorsanız, ilk isteğiniz için 30 ve `count` `offset` 0 olarak ayarlayabilirsiniz. Bir sonraki istekte, `offset` sorgu parametresini `nextOffset` değere ayarlarsınız.

> [!NOTE]
> Bu `TotalEstimatedAnswers` alan, geçerli sorgu için alabileceğiniz arama sonuçlarının toplam sayısının tahminidir.  Ve `count` `TotalEstimatedAnswers` parametrelerini ayarladığınızda, numara değişebilir. `offset` 
  
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Video öngörüleri edinin](video-insights.md)
