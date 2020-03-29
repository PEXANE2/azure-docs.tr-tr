---
title: Arama sonuçlarını görüntülemek için sıralamaları kullanma - Bing Web Arama API
titleSuffix: Azure Cognitive Services
description: Bing Web Arama API'sinden arama sonuçlarını görüntülemek için sıralamayı nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/17/2019
ms.author: scottwhi
ms.openlocfilehash: 677f6089f649aae720a6303a7e1512e3c7ebeca7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66390130"
---
# <a name="how-to-use-ranking-to-display-bing-web-search-api-results"></a>Bing Web Arama API sonuçlarını görüntülemek için sıralama nasıl kullanılır?  

Her arama yanıtı, arama sonuçlarını nasıl görüntülemeniz gerektiğini belirten bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse) yanıtı içerir. Sıralama yanıt grupları, geleneksel bir arama sonuçları sayfası için ana satır içeriğine ve kenar çubuğu içeriğine göre sonuçlar doğurur. Sonuçları geleneksel bir ana çizgi ve kenar çubuğu biçiminde görüntülemezseniz, ana hat içeriğinin kenar çubuğu içeriğinden daha yüksek görünürlük sağlamanız gerekir.  

Her grup içinde (ana hat veya kenar çubuğu), [Öğeler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankinggroup-items) dizisi içeriğin görünmesi gereken sırayı tanımlar. Her öğe, bir yanıt içinde sonucu tanımlamak için aşağıdaki iki yolu sağlar.  

-   `answerType`ve `resultIndex` — `answerType` Alan yanıtı (örneğin, Web sayfası veya `resultIndex` Haberler) tanımlar ve yanıt içinde bir sonucu tanımlar (örneğin, bir haber makalesi). Dizin sıfır tabanlıdır.  

-   `value`— `value` Alan, yanıt içinde bir yanıtın veya sonucun kimliğiyle eşleşen bir kimlik içerir. Yanıt veya sonuçlar kimliği içerir, ancak her ikisini birden içermez.  

Sıralama kimliğini yalnızca bir yanıtın kimliğiyle veya sonuçlarından biriyle eşleştirmeniz gerektiğinden, kimliği kullanmak daha kolaydır. Yanıt nesnesi `id` bir alan içeriyorsa, tüm yanıt sonuçlarını birlikte görüntüleyin. Örneğin, nesne `News` alanı içeriyorsa, `id` tüm haber makalelerini birlikte görüntüleyin. `News` Nesne alanı içermiyorsa, `id` her haber makalesi `id` bir alan içerir ve sıralama yanıtı haber makalelerini diğer yanıtların sonuçlarıyla karıştırır.  

Kullanarak `answerType` ve `resultIndex` biraz daha karmaşıktır. Görüntülenecek `answerType` sonuçları içeren yanıtı tanımlamak için kullanırsınız. Ardından, sonucu `resultIndex` görüntülemek için yanıtın sonuçlarını dizine dizine ekleyebilirsiniz. (Değer `answerType` [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#searchresponse) nesnesindeki alanın adıdır.) Tüm yanıt sonuçlarını birlikte görüntülemeniz gerekiyorsa, sıralama yanıt öğesi `resultIndex` alanı içermez.  

## <a name="ranking-response-example"></a>Sıralama yanıtı örneği

Aşağıdaki bir örnek [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse)gösterir. Web yanıtı bir `id` alan içermedığından, sıralamaya göre tüm web sayfalarını ayrı ayrı `id` görüntülersiniz (her web sayfası bir alan içerir). Görüntüler, videolar ve ilgili aramalar yanıtları alanı `id` içerdiğinden, bu yanıtların her birinin sonuçlarını sıralamaya göre birlikte görüntülersiniz.

```json
{  
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
        "totalEstimatedMatches" : 835000,
        "value" : [
            {
                "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
                "name" : "Motor Sports - Live at the race track ...",
                "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                "displayUrl" : "www.contoso.com\/usa\/eventsandracing\/motorsport",
                "snippet" : "Here you will find detailed information about racing...",
                "deepLinks" : [{
                    "name" : "Customer Racing",
                    "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                    "snippet" : "Customer racing news; General news..."
            },
            . . .  
        ]  
    }],  
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "2016 Supercar Wallpapers",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2017-03-25T11:14:00",
                "contentUrl" : "http:\/\/www.contoso.com\/wall...",
                "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "contentSize" : "373283 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "http:\/\/www.contoso.com\/lmp-...",
                "width" : 1920,
                "height" : 1080,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "insightsSourcesSummary" : {
                    "shoppingSourcesCount" : 0,
                    "recipeSourcesCount" : 0
                }
            },
            . . .  
        ]  
    },  
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [
            {
                "text" : "vintage racing teams",
                "displayText" : "vintage racing teams",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2..."
            },
            . . .  
        ]  
    },  
    "videos" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/videos...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "Why We Race",
                "description" : "A new era begins in motorsports this weekend...",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2...",
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.Vo1...",
                "datePublished" : "2014-01-25T16:31:48",
                "publisher" : [
                    {
                        "name" : "Fabrikam"
                    }
                ],
                "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=oL...",
                "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "encodingFormat" : "mp4",
                "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=oLAZgD...",
                "width" : 480,
                "height" : 360,
                "duration" : "PT2M42S",
                "motionThumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OM...",
                "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www.you...<\/iframe>",
                "allowHttpsEmbed" : true,
                "viewCount" : 47325,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "allowMobileEmbed" : true,
                "isSuperfresh" : false
            },
            . . .  
        ]  
    },  
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 2,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2"
                }
            },
            {
                "answerType" : "Videos",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 3,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 4,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 5,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5"
                }
            }]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}  
```  

Bu sıralama yanıtına göre, ana satır aşağıdaki arama sonuçlarını görüntüler:  

-   İlk web sayfası sonucu
-   Tüm görüntüler  
-   İkinci ve üçüncü web sayfası sonuçları  
-   Tüm videolar  
-   4, 5 ve 6 web sayfası sonuçları  

Kenar çubuğu aşağıdaki arama sonuçlarını görüntüler:  

-   İlgili tüm aramalar  


## <a name="next-steps"></a>Sonraki adımlar

Derecelendirilmemiş sonuçların tanıtımı hakkında bilgi için, [sıralamada olmayan yanıtları teşvik etme'ye](./filter-answers.md#promoting-answers-that-are-not-ranked)bakın.

Yanıttaki sıralanan yanıt sayısını sınırlama hakkında bilgi için [bkz.](./filter-answers.md#limiting-the-number-of-answers-in-the-response)

Sonuçları görüntülemek için sıralamayı kullanan bir C# örneği için [C# sıralama öğreticisine](./csharp-ranking-tutorial.md)bakın.
