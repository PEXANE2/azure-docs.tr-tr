---
title: Görüntü öngörüleri alın - Bing Resim Arama API'sı
titleSuffix: Azure Cognitive Services
description: Resim hakkında daha fazla bilgi edinmek için Bing Resim Arama API'sini nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 0BCD936E-D4C0-472D-AE40-F4B2AB6912D5
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.openlocfilehash: f84c6329c2a4dd0a9ad9e81f3700c9e31de95a2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68883424"
---
# <a name="get-image-insights-with-the-bing-image-search-api"></a>Bing Resim Arama API'si ile görüntü öngörüleri edinin

> [!IMPORTANT]
> Görüntü öngörüleri almak için /görüntüleri/ayrıntıları bitiş noktasını kullanmak yerine, daha kapsamlı öngörüler sağladığından [Görsel Arama'yı](../bing-visual-search/overview.md) kullanmalısınız.


Her resim, görüntü hakkında bilgi almak için kullanabileceğiniz bir öngörü belirteci içerir. Örneğin, ilgili resimlerin, resmiiçeren web sayfalarının veya resimde gösterilen ürünü satın alabileceğiniz satıcıların listesini alabilirsiniz.  

Bir görüntü hakkında bilgi edinmek için, yanıtta görüntünün [görüntüinsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image-imageinsightstoken) belirteci yakalamak.

```json
"value" : [{
        . . .
        "name":"sailing dinghy.jpg",
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "insightsSourcesSummary" : {
            "shoppingSourcesCount" : 9,
            "recipeSourcesCount" : 0
        },
        . . .
}],
```

Ardından, Resim Ayrıntıları bitiş noktasını arayın ve [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresini 'deki `imageInsightsToken`belirteç'e ayarlayın.  

Almak istediğiniz öngörüleri belirtmek için sorgu `modules` parametresini ayarlayın. Tüm sezgileri almak `modules` `All`için, ayarlayın. Yalnızca resim yazısı ve koleksiyon istatistiklerini almak `modules` için. `Caption%2CCollection` Olası öngörülerin tam listesi için [modüllere](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested)bakın. Tüm görüntüler için tüm öngörüler mevcut değildir. Yanıt, varsa istediğiniz tüm öngörüleri içerir.

Aşağıdaki örnek, önceki resim için tüm kullanılabilir öngörüleri ister.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-insights-of-a-known-image"></a>Bilinen bir görüntünün içgörülerini alma

Eğer size anlayışlar almak istediğiniz bir resmin URL'si varsa, görüntüyü belirtmek için [öngörülerToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) parametresi yerine [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imgurl) sorgu parametresini kullanın. Veya, resim dosyası varsa, bir POST isteğinin gövdesinde görüntünün ikili gönderebilirsiniz. POST isteği kullanıyorsanız, `Content-Type` üstbilgi `multipart/data-form`'ye ayarlanmalıdır. Her iki seçenekle de görüntünün boyutu 1 MB'ı geçemez.  

Görüntünün URL'si varsa, aşağıdaki örnek, bir resmin öngörülerini nasıl isteyeceğinizi gösterir.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-all-image-insights"></a>Tüm görüntü öngörülerini alma  

Görüntünün tüm öngörülerini istemek `All` [için, modülsorgu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini . İlgili aramaları almak için isteğin kullanıcının sorgu dizesini içermesi gerekir. Bu örnek, görüntüyü belirtmek için [öngörülerToken'in](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) kullanılmasını gösterir.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Üst düzey nesne, [Görüntüler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) [nesnesi yerine ImageInsightsResponse nesnesidir.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse)  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "bcid_3297E6A54E4787A5F51C49D9BA342B9A*ccid_Fe2Hx...",
    "bestRepresentativeQuery" : {
        "text" : "Sailing Dinghy",
        "displayText" : "Sailing Dinghy",
        "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing+Dinghy...",
    },
    "pagesIncluding" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Powerboating Dublin, Dinghy Sailing Courses...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
                "datePublished" : "2017-01-20T00:41:00.0000000Z",
                "contentUrl" : "http:\/\/www.contoso.ie\/content...",
                "hostPageUrl" : "http:\/\/www.contoso.ie\/powerboating...",
                "contentSize" : "59063 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "www.contoso.ie\/powerboating...",
                "width" : 800,
                "height" : 600,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 225
                },
                "imageInsightsToken" : "ccid_pHjQIA0x*mid_17F61B1316A39C92214...",
                "imageId" : "17F61B1316A39C922143FFDE9DFB5B0FB41171",
                "accentColor" : "0997C2"
            },
            . . .
        ]
    },
    "relatedSearches" : {
        "value" : [
            {
                "text" : "Sailing Fun",
                "displayText" : "Sailing Fun",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing...",
                "thumbnail" : {
                    "url" : "https:\/\/tse1.mm.bing.net\/th?q=Sailing+Fun..."
                }
            },
            . . .
        ]
    },
    "visuallySimilarImages" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Weekend On the Water",
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2010-09-05T12:00:00.0000000Z",
                "contentUrl" : "http:\/\/1.bp.contoso.com\/_dc_6...",
                "hostPageUrl" : "http:\/\/contoso.com\/2010...",
                "contentSize" : "203806 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "contoso.com\/2010...",
                "width" : 1600,
                "height" : 1249,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 234
                },
                "imageInsightsToken" : "ccid_Jg1Kwuc4*mid_5B7DA43976D3A422...",
                "imageId" : "5B7DA43976D3A422BA679A3AB019BB52C08DBC",
                "accentColor" : "0B2543"
            },
            . . .
        ]
    },
    "imageTags" : {
        "value" : [
            {
                "name" : "sail boat"
            },
            . . .
        ]
    }
}
```

## <a name="recognizing-entities-in-an-image"></a>Görüntüdeki varlıkları tanıma  

Varlık tanıma özelliği, bir görüntüdeki varlıkları, şu anda yalnızca kişileri tanımlar. Görüntüdeki varlıkları tanımlamak `RecognizedEntities` [için, modülsorgu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini .  

> [!NOTE]
> Bu modülü başka bir modülle belirtemeyebilirsiniz. Bu modülü diğer modüllerle birlikte belirtirseniz, yanıt tanınan varlıkları içermez.  

Aşağıda [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imgurl) parametresini kullanarak görüntünün nasıl belirtiliz olduğu gösterilmektedir. SORGU parametrelerini URL'ye kodlamayı unutmayın.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=faith+hill&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Görüntü iki kişi içerdiğinden, yanıt her kişi için bir bölge tanımlar. Bu durumda, insanlar CelebrityAnnotations ve CelebRecognitionAnnotations gruplarında tanındı. Bing, her gruptaki kişileri, orijinal görüntüdeki kişiyle eşleşme olasılığına göre listeler. Liste güven azalan sırada. CelebRecognitionAnnotations grubu, eşleşmenin doğru olduğuna dair en yüksek düzeyde güven sağlar.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "recognizedEntityGroups" : {
        "value" : [
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebRecognitionAnnotations"
            },
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebrityAnnotations"
            }
        ]
    }
}
```

Alan, `region` Bing'in varlığı tanıdığı görüntünün alanını tanımlar. İnsanlar için bölge kişinin yüzünü temsil eder.  

Dikdörtgenin değerleri orijinal görüntünün genişliğine ve yüksekliğine göredir ve 0,0 ile 1,0 aralığındadır. Örneğin, görüntü 300x200 ise ve bölgenin üst, sol köşe noktası (10, 20) ve alt, sağ köşe noktası (290, 150) ise, normalleştirilmiş dikdörtgen:  

-   Sol: 10 / 300 = 0.03333...  
-   Üst: 20 / 200 = 0,1  
-   Sağ: 290 / 300 = 0.9667...  
-   Alt: 150 / 200 = 0,75  

Bing'in sonraki öngörülerde döndürdettiği bölgeyi kullanabilirsiniz. Örneğin, tanınan varlığın görsel olarak benzer görüntülerini almak için. Daha fazla bilgi için Görsel Olarak Benzer ve Varlık Tanıma Modülleri ile kullanmak üzere Kırpma Görüntüleri'ne bakın. Aşağıda, bölge alanları arasındaki eşleme ve görüntüleri kırpmak için kullanacağınız sorgu parametreleri gösterilmektedir.  

-   [Cal'a](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cal) sol haritalar  
-   [Kedi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cat) için en iyi haritalar  
-   [Arabaya](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#car) doğru haritalar  
-   [Kabin](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cab) için alt haritalar  

## <a name="finding-visually-similar-images"></a>Görsel olarak benzer görüntüleri bulma  

Görsel olarak orijinal görüntüye benzer görüntüleri bulmak [için, modülsorgu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini SimilarImages olarak ayarlayın.  

Aşağıdaki istek, görsel olarak benzer görüntülerin nasıl alınır olduğunu gösterir. İstek, özgün görüntüyü tanımlamak için [öngörülerToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresini kullanır. Alaka düzeyini artırmak için, kullanıcının sorgu dizesini eklemeniz gerekir.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?insightsToken=mid_68364D764J...&modules=SimilarImages&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```


Aşağıda, bir önceki isteğin yanıtı gösterilmektedir.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarImages" : {
        "value" : [
            {
                "name" : "typical Hawaiian Sunset! :) | Scenes of Hawaii",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detailv2...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.Mda2a86...",
                 . . .
            }
        ]
    }
```

## <a name="cropping-images-to-use-with-visually-similar-and-entity-recognition-modules"></a>Görsel olarak benzer ve varlık tanıma modülleriyle kullanılacak görüntüleri kırpma  

Bing'in görüntülerin görsel olarak benzer olup olmadığını belirlemek veya varlık tanıma gerçekleştirmek için kullandığı görüntünün bölgesini belirtmek için [cal,](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cal) [cat,](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cat) [cab](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cab)ve [araç](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#car) sorgu parametrelerini kullanın. Varsayılan olarak, Bing görüntünün tamamını kullanır.  

Parametreler, Bing'in karşılaştırma için kullandığı bölgenin üst, sol ve alt, sağ köşesini belirtir. Değerleri özgün görüntünün genişliği nin ve yüksekliğinin kesirleri olarak belirtin. Kesirli değerler üstte (0,0, 0,0) ile başlar ve sağ alt köşede (1,0, 1,0) ile biter. Örneğin, üst, sol köşenin üstten aşağı doğru dörtte bir, sol taraftan gelen yolun dörtte biri `cal` 0,25 `cat` ve 0,25 olarak ayarlanan dörtte birini belirtir.  

Aşağıdaki arama sırası kırpma bölgesini belirtme nin etkisini gösterir. İlk arama kırpma içermez ve Bing görüntünün ortasında yan yana duran iki kişiyi tanır.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Yanıt, tanınan iki varlığı gösterir.  

```json
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value": [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                },  
                {  
                    "region" : {  
                        "left" : 0.25,  
                        "top" : 0.2488889,  
                        "right" : 0.4466667,  
                        "bottom" : 0.5111111  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Marcus Appel",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
            "name" : "CelebRecognitionAnnotations"
        }]
    }  
}  
```  

İkinci çağrı, görüntüyü dikey olarak ortadan aşağı yaslar ve Bing görüntünün sağ tarafındaki tek bir kişiyi tanır.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Yanıt, tanınan bir varlığı gösterir.  

```json  
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value" : [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
                "name" : "CelebRecognitionAnnotations"  
            }
        ]
    }
}  
```  

## <a name="finding-visually-similar-products"></a>Görsel olarak benzer ürünleri bulma  

Orijinal resimde bulunan ürünlere görsel olarak benzeyen ürünler içeren görüntüleri bulmak [için, modülsorgu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) parametresini SimilarProducts olarak ayarlayın.  

Aşağıdaki istek, görsel olarak benzer ürünlerin görüntülerini nasıl elde edilebildiğini gösterir. İstek, önceki bir istekte döndürülen özgün görüntüyü tanımlamak için [öngörülerToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresini kullanır. Alaka düzeyini artırmak için, kullanıcının sorgu dizesini eklemeniz gerekir.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=anne+klein+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Yanıt benzer bir ürünün görüntüsünü içerir ve kaç satıcının ürünü çevrimiçi olarak sunduğunu, ürün derecelendirmeleri olup `aggregateOffer` olmadığını ve bulunan en düşük fiyatı gösterir (alana bakın).  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarProducts" : {
        "value" : [
            {
                "name" : "Sequin One-Shoulder Twist-Drape Dress",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=de...",  
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP.M85bdee...",  
                . . .
            },
            . . .
        ]
    }
}
```

Ürünü çevrimiçi olarak sunan satıcıların listesini almak için [(teklifSayısı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) alanına bakın), API'yi yeniden arayın ve ShoppingSources'e ayarlayın. `modules` Ardından, sorgu `insightsToken` parametresini ürün özeti görüntüsünde bulunan belirteç olarak ayarlayın.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=ShoppingSources&insightsToken=ccid_hb3uRvUk*mid_BF5C252A47F2C765...&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Aşağıda önceki isteğe yanıt vereme işlenir.  

```json  
{  
    "_type" : "ImageInsights",  
    "shoppingSources" : {  
        "offers" : [{  
            "url" : "http:\/\/www.contoso.com\/dp\/B00O...",  
            "seller" : {  
                "name" : "Contoso",  
                "image" : {  
                    "url" : "https:\/\/tse3.mm.bing.net\/th?id=A10d50fe..."  
                }  
            },  
            "price" : 126.87,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        },  
        {  
            "url" : "http:\/\/www.adatum.com\/product\/heritage...\/",  
            "seller" : {  
                "name" : "fabrikam.com"  
            },  
            "price" : 495,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        }]  
    }  
}  
```
