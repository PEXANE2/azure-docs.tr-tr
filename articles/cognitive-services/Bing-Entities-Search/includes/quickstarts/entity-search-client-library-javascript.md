---
title: Bing Entity Search JavaScript istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: ac2f83c824014e16cfbe9ab18483b914ed8b077d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79136770"
---
JavaScript için Bing Entity Search istemci kitaplığı olan varlıkları aramaya başlamak için bu hızlı başlatmayı kullanın. Bing Entity Search çoğu programlama diliyle uyumlu bir REST API'sine sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [Düğüm.js için Bing Varlık Arama SDK](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

Bing Entity Search SDK'yı yüklemek için:

1. Geliştirme `npm install ms-rest-azure` ortamınızda çalıştırın.
2. Geliştirme `npm install @azure/cognitiveservices-entitysearch` ortamınızda çalıştırın.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ekleyin.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. Abonelik anahtarınızı `CognitiveServicesCredentials` kullanma örneği oluşturun. Ardından, arama istemcisinin bir örneğini oluşturun.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>İstek gönderme ve yanıt alma

1. Bir varlıklar arama isteği `entitiesOperations.search()`ile gönder. Yanıt aldıktan `queryContext`sonra, döndürülen sonuç sayısını ve ilk sonucun açıklamasını yazdırın.

    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md)