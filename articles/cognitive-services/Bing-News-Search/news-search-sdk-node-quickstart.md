---
title: "Hızlı başlangıç: node. js için SDK 'Yı kullanarak bir haber araması gerçekleştirme-Bing Haber Arama"
titleSuffix: Azure Cognitive Services
description: Node. js için Bing Haber Arama SDK 'sını kullanarak haberleri aramak ve yanıtı işlemek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 832ce90db1d4eac0ef8db87c10a5bc2a1658216d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75383201"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Hızlı başlangıç: node. js için Bing Haber Arama SDK ile bir haber araması gerçekleştirme

Node. js için Bing Haber Arama SDK 'Sı ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/)

Bing Haber Arama SDK kullanarak bir konsol uygulaması ayarlamak için:
1. Geliştirme ortamınızda `npm install ms-rest-azure` çalıştırın.
2. Geliştirme ortamınızda `npm install azure-cognitiveservices-newssearch` çalıştırın.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

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
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
