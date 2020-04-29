---
title: Bing Haber Arama JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: 858e6b9e0e40ab988a4cdf04b31580c1ca28d40a
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79503898"
---
JavaScript için Bing Haber Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/)

Bing Haber Arama istemci kitaplığını kullanarak bir konsol uygulaması ayarlamak için:
1. Geliştirme `npm install ms-rest-azure` ortamınızda çalıştırın.
2. Geliştirme `npm install azure-cognitiveservices-newssearch` ortamınızda çalıştırın.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. `CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun. Abonelik anahtarınız ve bir arama terimi için değişkenler oluşturun.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let search_term = 'Winter Olympics'
    ```

2. istemcinin örneğini oluşturma:
    
    ```javascript
    const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
    let client = new NewsSearchAPIClient(credentials);
    ```

## <a name="send-a-search-query"></a>Arama sorgusu gönder

1. Bir sorgu terimiyle aramak için istemcisini kullanın, bu durumda "Kış Olimpiyatları":
    
    ```javascript
    client.newsOperations.search(search_term).then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

Kod konsola `result.value` öğelerini yazdırır ve metin ayrıştırması gerçekleştirmez. Varsa sonuçlar kategorilere ayrılmış şekilde şunları içerir:

- `_type: 'NewsArticle'`
- `_type: 'WebPage'`
- `_type: 'VideoObject'`
- `_type: 'ImageObject'`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-news-search-single-page-app.md)
