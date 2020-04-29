---
title: Bing Varlık Arama JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: ac2f83c824014e16cfbe9ab18483b914ed8b077d
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79136770"
---
JavaScript için Bing Varlık Arama istemci kitaplığıyla varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [Node. js için Bing varlık arama SDK 'sı](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

Bing Varlık Arama SDK 'sını yüklemek için:

1. Geliştirme `npm install ms-rest-azure` ortamınızda çalıştırın.
2. Geliştirme `npm install @azure/cognitiveservices-entitysearch` ortamınızda çalıştırın.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ekleyin.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. Abonelik anahtarınızı `CognitiveServicesCredentials` kullanarak bir örnek oluşturun. Ardından, bununla birlikte arama istemcisinin bir örneğini oluşturun.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>İstek gönderme ve yanıt alma

1. İle `entitiesOperations.search()`bir varlık arama isteği gönderin. Bir yanıt aldıktan sonra, döndürülen sonuç sayısını `queryContext`ve ilk sonucun açıklamasını yazdırın.

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