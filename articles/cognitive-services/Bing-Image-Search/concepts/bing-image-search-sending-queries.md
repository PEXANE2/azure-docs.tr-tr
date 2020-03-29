---
title: Resim arama sorgularını özelleştirin ve önerin - Bing Resim Arama API
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'sine gönderdiğiniz arama sorgularını özelleştirme hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: d833b017004365e9dad7241e360f42ff41a55883
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67542750"
---
# <a name="customize-and-suggest-image-search-queries"></a>Resim arama sorgularını özelleştirme ve önerme

Sorguları özelleştirmeyi ve Bing Resim Arama API'sine göndermek için arama terimleri önermeyi öğrenmek için bu makaleyi kullanın.

## <a name="suggest-search-terms"></a>Arama terimleri öner

Uygulamanızda arama terimlerinin girildiği bir arama kutusu varsa, deneyimi geliştirmek için [Bing Autosuggest API'sini](../../bing-autosuggest/get-suggested-search-terms.md) kullanabilirsiniz. API önerilen arama terimlerini gerçek zamanlı olarak görüntüleyebilir. API, kısmi arama terimlerini ve Bilişsel Hizmetleri temel alan önerilen sorgu dizelerini döndürür.

## <a name="pivot-the-query"></a>Sorguyu döndür

Bing özgün arama sorgusunu segmente edebilirse, döndürülen [Görüntüler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesnesi . `pivotSuggestions` Pivot önerileri kullanıcıya isteğe bağlı arama terimleri olarak görüntülenebilir. Örneğin, özgün sorgu *Microsoft Surface*ise Bing sorguyu *Microsoft* ve *Surface* olarak bölümlere ayırabilir ve her biri için önerilen pivotlar sağlayabilir. Bu öneriler kullanıcıya isteğe bağlı sorgu terimleri olarak görüntülenebilir.

Aşağıdaki örnek, *Microsoft Surface*için pivot önerilerini gösterir:  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

`pivotSuggestions` alanında özgün sorgunun ayrıldığı parçaların (özetler) listesi bulunur. Her bir özet terim için gelen yanıtta önerilen sorguları içeren [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj) nesnelerinin listesi yer alır. Alan `text` önerilen sorguyu içerir. Alan, `displayText` özgün sorgudaki pivotun yerini alan terimi içerir. Bir örnek Yüzeyin Çıkış Tarihidir.

Pivot sorgu dizesi kullanıcının aradığı şeyse, pivot sorgu dizelerini görüntülemek için alanları ve alanları `text` `thumbnail` kullanın. URL'yi veya URL'yi kullanarak `webSearchUrl` küçük `searchLink` resmi ve metni tıklanabilir hale getirin. Kullanıcıyı Bing arama sonuçlarına göndermek için kullanın. `webSearchUrl` Kendi sonuç sayfanızı sağlıyorsanız, 'yi kullanın. `searchLink`

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expand-the-query"></a>Sorguyu genişletme

Bing özgün aramayı daraltmak için sorguyu genişletebiliyorsa [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesnesi `queryExpansions` alanını içerir. Örneğin, sorgu Microsoft *Surface*ise, genişletilmiş sorgular olabilir:
- Microsoft Surface **Pro 3**.
- Microsoft Yüzey **RT**.
- Microsoft Surface **Phone**.
- Microsoft Surface **Hub**.

Aşağıdaki örnekte *Microsoft Surface* için genişletilmiş sorgular gösterilmektedir.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

`queryExpansions` alanı [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj) nesnelerinin listesini içerir. Alan `text` genişletilmiş sorgu içerir. Alan `displayText` genişletme terimini içerir. Genişletilmiş sorgu dizesi kullanıcının aradığı şeyse, genişletilmiş sorgu dizelerini görüntülemek için alanları ve alanları `text` `thumbnail` kullanın. URL'yi veya URL'yi kullanarak `webSearchUrl` küçük `searchLink` resmi ve metni tıklanabilir hale getirin. Kullanıcıyı Bing arama sonuçlarına göndermek için kullanın. `webSearchUrl` kendi sonuç sayfanızı sağlarsanız, kullanın. `searchLink`

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bing Resim Arama API'sini daha önce denemediyseniz, hızlı bir [başlangıç](../quickstarts/csharp.md)deneyin. Daha karmaşık bir şey arıyorsanız, [tek sayfalık](../tutorial-bing-image-search-single-page-app.md)bir web uygulaması oluşturmak için öğreticiyi deneyin.
