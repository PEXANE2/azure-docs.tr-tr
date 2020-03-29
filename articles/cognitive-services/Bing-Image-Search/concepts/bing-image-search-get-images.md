---
title: Web'den resim alın - Bing Resim Arama API'sı
titleSuffix: Azure Cognitive Services
description: Web'de alakalı görüntüleri aramak ve almak için Bing Resim Arama API'sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 309bbca762149f8804742d9ef02d4c3e8dfcdc6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67542759"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Bing Resim Arama API'si ile web'den resim alma

Bing Image Search REST API'sini kullandığınızda, aşağıdaki GET isteğini göndererek web'den arama teriminizle ilgili görüntüler alabilirsiniz:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Url kodlanmış arama teriminiz için [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) sorgu parametresini kullanın. Örneğin, *yelkenli dinghies*girerseniz `q` `sailing+dinghies` , `sailing%20dinghies`ayarlayın veya .

> [!IMPORTANT]
> * Tüm istekler istemciden değil, bir sunucudan yapılmalıdır.
> * Bing arama API'lerinden herhangi birini ilk kez arıyorsanız, istemci kimliği üstbilgisini eklemeyin. Yalnızca daha önce kullanıcı ve aygıt birleşimi için istemci kimliği döndüren bir Bing API'sını aradıysanız istemci kimliğini ekleyin.

## <a name="get-images-from-a-specific-web-domain"></a>Belirli bir web etki alanından resim alma

Belirli bir etki alanındaki görüntüleri almak için [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> İşleç kullanan sorgulara verilen yanıtlar, [güvenli Arama](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch) ayarından bağımsız olarak yetişkinlere uygun içerik içerebilir. `site:` Yalnızca `site:` etki alanında içeriğin farkındaysanız kullanın.

## <a name="filter-images"></a>Görüntüleri filtreleme

 Varsayılan olarak, Resim Arama API sorguile ilgili tüm görüntüleri döndürür. Bing'in döndürdettiği görüntüleri filtrelemek istiyorsanız (örneğin, yalnızca saydam arka plana veya belirli bir boyuta sahip görüntüleri döndürmek için), aşağıdaki sorgu parametrelerini kullanın:

* [boy](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#aspect)—Görüntüleri en boy oranına göre filtreleyin (örneğin, standart veya geniş ekran görüntüleri).
* [renk](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#color)—Görüntüleri baskın renge veya siyah beyaza göre filtreleyin.
* [tazelik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#freshness)—Görüntüleri yaşa göre filtreleyin (örneğin, Bing tarafından geçen hafta keşfedilen görüntüler).
* [yükseklik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#height), [genişlik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#width)—Görüntüleri genişliğe ve yüksekliğe göre filtreleyin.
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagecontent)—Görüntüleri içeriğe göre filtreleyin (örneğin, yalnızca bir kişinin yüzünü gösteren görüntüler).
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype)—Görüntüleri türüne göre filtreleyin (örneğin, küçük resim, animasyonlu GIF'ler veya saydam arka planlar).
* [lisans](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license)—Görüntüleri siteyle ilişkili lisans türüne göre filtreleyin.
* [boyutu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#size)—Görüntüleri 200x200 piksele kadar küçük görüntüler gibi boyutlara göre filtreleyin.

Belirli bir etki alanındaki görüntüleri almak için [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

Aşağıdaki örnek, Bing'in geçen hafta keşfettiği ContosoSailing.com küçük görüntülerin nasıl alındığını gösterir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Bing Resim Arama yanıt biçimi

Bing'den gelen yanıt iletisi, Bilişsel Hizmetler'in sorguyla alakalı olduğunu belirlediği görüntülerin listesini içeren bir [Görüntüler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) yanıtı içerir. Listedeki her [Resim](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) nesnesi resim hakkında aşağıdaki bilgileri içerir: URL, boyutu, boyutları, kodlama biçimi, resmin küçük resmine bir URL ve küçük resmin boyutları.

> [!NOTE]
> * Görüntüler yanıtta verilen sırada görüntülenmelidir.
> * URL biçimleri ve parametreleri önceden haber verilmeden değiştirilebilir olduğundan, tüm URL'leri olduğu gibi kullanın. Belirtildiği durumlar dışında URL biçimine veya parametrelerine bağımlılık yapmamanız gerekir.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Bing Resim Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Yanıtın `totalEstimatedMatches` alanı, görüntülenebilecek tahmini görüntü sayısını içerir. Görüntülerin geri kalanında nasıl sayfaya atılanın, [Bkz.](../paging-images.md)

## <a name="next-steps"></a>Sonraki adımlar

Bing Resim Arama API'sini daha önce denemediyseniz, hızlı bir [başlangıç](../quickstarts/csharp.md)deneyin. Daha karmaşık bir şey arıyorsanız, [tek sayfalık](../tutorial-bing-image-search-single-page-app.md)bir web uygulaması oluşturmak için öğreticiyi deneyin.
