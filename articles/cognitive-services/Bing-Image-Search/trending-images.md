---
title: Bing Resim Arama API'si popüler resimleri al
titleSuffix: Azure Cognitive Services
description: Günümüzün popüler görüntülerini Web 'den Bing Resim Arama API'si ile arayın.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66383428"
---
# <a name="get-trending-images-from-the-web"></a>Web 'den popüler resimleri al

Günümüzün popüler görüntülerini almak için aşağıdaki GET isteğini gönderin:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Popüler resimler API 'SI Şu anda yalnızca aşağıdaki pazarları desteklemektedir:  

- en-US (Ingilizce, Birleşik Devletler)  
- en-CA (Ingilizce, Kanada)  
- En-AU (Ingilizce, Avustralya)  
- zh-CN (Çince, Çin)

Yanıt, görüntüleri kategoriye göre listeleyen bir [Trendingimages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#trendingimages) nesnesi içerir. Kullanıcı deneyiminizdeki görüntüleri `title` gruplandırmak için kategorisini kullanın. Kategoriler günlük olarak değişebilir.  

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

Her kutucuk, ilgili görüntüleri almak için bir görüntü ve seçenek içerir. İlgili görüntüleri almak için, [resım arama API](./search-the-web.md) 'sini çağırmak ve ilgili `text` görüntüleri kendiniz göstermek için sorguyu kullanabilirsiniz. Ya da, içindeki `webSearchUrl` URL 'yi kullanarak ilgili görüntüleri içeren Bing resim arama sonuçları sayfasına kullanıcıyı ekleyebilirsiniz.

İlgili görüntüleri almak için Resim Arama API 'sini çağırırsanız, [kimlik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#id) sorgu PARAMETRESINI `id` alanındaki kimliğe ayarlayın. KIMLIĞI belirtmek yanıtın görüntüyü (yanıtta ilk görüntüdür) ve ilişkili görüntülerini içerdiğinden emin olur. Ayrıca, [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) sorgu parametresini `query` nesne `text` alanındaki metne ayarlayın.

Aşağıdaki örnek, önceki popüler görüntüler API yanıtında Mr. Smith 'in ilişkili görüntülerini almak için görüntü KIMLIĞININ nasıl kullanılacağını gösterir.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
