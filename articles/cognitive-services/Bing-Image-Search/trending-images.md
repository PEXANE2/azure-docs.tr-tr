---
title: Bing Resim Arama API'si ile popüler görüntüler elde edin
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API ile web'den günümüzün popüler görüntülerini arayın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 2936b94d7ba791b1a4e5a9b95aca3ca3ecdb5904
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66383428"
---
# <a name="get-trending-images-from-the-web"></a>Web'den popüler görüntüler alın

Günümüzün popüler görüntülerini almak için aşağıdaki GET isteğini gönderin:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Trending Images API şu anda yalnızca aşağıdaki pazarları destekler:  

- tr-US (İngilizce, Amerika Birleşik Devletleri)  
- tr-CA (İngilizce, Kanada)  
- tr-AU (İngilizce, Avustralya)  
- zh-CN (Çince, Çin)

Yanıt, görüntüleri kategoriye göre listeleyen bir [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#trendingimages) nesnesi içerir. Kullanıcı deneyiminizdeki `title` görüntüleri gruplandırmak için kategoriyi kullanın. Kategoriler her gün değişebilir.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Her döşeme, ilgili görüntüleri almak için bir görüntü ve seçenekler içerir. İlgili resimleri almak için, sorguyu `text` kullanarak [Resim Arama API'sini](./search-the-web.md) arayabilir ve ilgili görüntüleri kendiniz görüntüleyebilirsiniz. Veya, kullanıcıyı Bing'in ilgili resimleri içeren resim arama sonuçları sayfasına götürmek `webSearchUrl` için URL'yi kullanabilirsiniz.

İlgili görüntüleri almak için Resim Arama API'sını ararsanız, [kimlik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#id) sorgu `id` parametresini alandaki id'ye ayarlayın. Kimliğin belirtilmesi, yanıtın görüntüyü (yanıttaki ilk görüntüdür) ve ilgili görüntüleri içermesini sağlar. Ayrıca, [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) sorgu parametresini nesnenin `query` `text` alanındaki metne ayarlayın.

Aşağıdaki örnek, önceki Trending Images API yanıtında Bay Smith'in ilgili görüntülerini almak için görüntü kimliğinin nasıl kullanılacağını gösterir.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
