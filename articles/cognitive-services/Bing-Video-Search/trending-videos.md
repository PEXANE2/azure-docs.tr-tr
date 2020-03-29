---
title: Bing Video Arama API'sini kullanarak popüler videolar için web'de arama yapın
titleSuffix: Azure Cognitive Services
description: Popüler videolar için web'de arama yapmak için Bing Video Arama API'sını nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: ea6f79bf6c305ecc07b3e684ede15ad439039e7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68500606"
---
# <a name="get-trending-videos-with-the-bing-video-search-api"></a>Bing Video Arama API'si ile popüler videolar alın 

Bing Video Arama API'sı, web'de ve farklı kategorilerde günümüzün popüler videolarını bulmanızı sağlar. 

## <a name="get-request"></a>İstek AL

Bing Video Arama API'sinden günümüzün popüler videolarını almak için aşağıdaki GET isteğini gönderin:  
  
```cURL
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="market-support"></a>Pazar desteği

Aşağıdaki pazarlar popüler videoları destekler.  
 
-   tr-AU (İngilizce, Avustralya)  
-   tr-CA (İngilizce, Kanada)  
-   tr-GB (İngilizce, Büyük Britanya)  
-   en-ID (İngilizce, Endonezya)  
-   en-IE (İngilizce, İrlanda)  
-   tr-IN (İngilizce, Hindistan)  
-   en-NZ (İngilizce, Yeni Zelanda)  
-   tr-PH (İngilizce, Filipinler)  
-   en-SG (İngilizce, Singapur)  
-   tr-US (İngilizce, Amerika Birleşik Devletleri)  
-   tr-WW (İngilizce, Dünya çapında toplam kod)  
-   en-ZA (İngilizce, Güney Afrika)  
-   zh-CN (Çince, Çin)

## <a name="example-json-response"></a>Örnek JSON yanıtı  

Aşağıdaki örnek, kategori ye ve alt kategoriye göre listelenen popüler videoları içeren bir API yanıtı gösterir. Yanıt aynı zamanda en popüler popüler videolar olan ve bir veya daha fazla kategoriden gelebilecek banner videoları içerir.  

```json
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Video içgörüleri edinme](video-insights.md)