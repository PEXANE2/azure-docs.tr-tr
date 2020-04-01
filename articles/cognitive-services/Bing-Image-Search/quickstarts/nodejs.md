---
title: 'Hızlı başlangıç: Bing Image Search REST API ve Node.js kullanarak görüntüleri arayın'
titleSuffix: Azure Cognitive Services
description: JavaScript kullanarak Bing Image Search REST API'ye görüntü arama istekleri göndermek ve Bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 03/31/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 15757d606a846a2951bc5c15d8d5ef0dbfd7a2a1
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478601"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-nodejs"></a>Hızlı başlangıç: Bing Image Search REST API ve Node.js kullanarak görüntüleri arayın

Bing Resim Arama API'sine arama istekleri göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu JavaScript uygulaması API'ye bir arama sorgusu gönderir ve sonuçlardaki ilk resmin URL'sini görüntüler. Bu uygulama Javascript'te yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir.

Bu örnek için kaynak kodu, ek hata işleme ve ek açıklamalar ile [GitHub kullanılabilir.](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingImageSearchv7Quickstart.js)

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [JavaScript İstek Kitaplığı](https://github.com/request/request)  

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Ayrıca bakınız [Bilişsel Hizmetler Fiyatlandırma - Bing Arama API.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE’de veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve katılık ve https gereksinimlerini ayarlayın.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. API uç noktası, görüntü API’si arama yolu, abonelik anahtarınız ve arama teriminiz için değişkenler oluşturun. `host`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir.

    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/images/search';
    let term = 'tropical ocean';
    ```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturun.

1. API isteğine yönelik bir arama URL’sini biçimlendirmek için son adımdaki değişkenleri kullanın. Arama teriminiz API'ye gönderilmeden önce URL kodlanmış olmalıdır.

    ```javascript
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
        'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    ```

2. API’ye sorgunuzu göndermek için istek kitaplığını kullanın. `response_handler`, sonraki bölümde tanımlanır.
    ```javascript
    let req = https.request(request_params, response_handler);
    req.end();
    ```

## <a name="handle-and-parse-the-response"></a>Yanıtı işleme ve ayrıştırma

1. HTTP çağrısı (`response`) alan `response_handler` adlı bir işlevi parametre olarak tanımlayın. Bu işlev içinde aşağıdaki adımları yapın:

    1. JSON yanıtının gövdesini içerecek bir değişken tanımlayın.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. **Veri** işareti çağrıldığında yanıtın gövdesini depolama
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Bir **bitiş** bayrağı sinyali verildiğinde, JSON yanıtından ilk sonucu alın. İlk görüntünün URL'sini ve döndürülen toplam görüntü sayısını yazdırın.

        ```javascript
        response.on('end', function () {
            let firstImageResult = imageResults.value[0];
            console.log(`Image result count: ${imageResults.value.length}`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image web search url: ${firstImageResult.webSearchUrl}`);
         });
        ```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Bing Resim Arama API'sinden yanıtlar JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalık bir uygulama oluşturma](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) 
* Bing Arama API'leri için [fiyatlandırma ayrıntıları.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) 
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
