---
title: Bing Video Arama API'si kullanarak Web 'de popüler videolar için arama yapın
titleSuffix: Azure Cognitive Services
description: Web 'i popüler videolar için aramak üzere Bing Video Arama API'si nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 8232af6cb409628a1066a044a196777ddc46348f
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098977"
---
# <a name="get-trending-videos-with-the-bing-video-search-api"></a>Bing Video Arama API'si popüler videolar alın 

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Bing Video Arama API'si, günümüzün popüler videolarını Web üzerinden ve farklı kategorilerde bulmanıza olanak sağlar. 

## <a name="get-request"></a>GET isteği

Günümüzün popüler videolarını Bing Video Arama API'si almak için aşağıdaki GET isteğini gönderin:  
  
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

Aşağıdaki pazarlar, popüler videoları destekler.  
 
-   En-AU (Ingilizce, Avustralya)  
-   en-CA (Ingilizce, Kanada)  
-   en-GB (Ingilizce, Büyük Britanya)  
-   En-KIMLIĞI (Ingilizce, Endonezya)  
-   en-IE (Ingilizce, Irlanda)  
-   En-ın (Ingilizce, Hindistan)  
-   En-NZ (Ingilizce, Yeni Zelanda)  
-   En-PH (Ingilizce, Filipinler)  
-   En-SG (Ingilizce, Singapur)  
-   en-US (Ingilizce, Birleşik Devletler)  
-   En-WW (Ingilizce, dünya çapında toplam kod)  
-   En-ZA (Ingilizce, Güney Afrika)  
-   zh-CN (Çince, Çin)

## <a name="example-json-response"></a>Örnek JSON yanıtı  

Aşağıdaki örnek, kategori ve alt kategori ile listelenmiş popüler videolar içeren bir API yanıtı gösterir. Yanıt Ayrıca en popüler popüler videolar olan başlık videoları içerir ve bir veya daha fazla kategoriden gelebilir.  

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