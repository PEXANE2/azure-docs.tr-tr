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
ms.custom: devx-track-javascript
ms.openlocfilehash: f5a3b48fd6be48d468b66009a77de100962be10d
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405084"
---
JavaScript için Bing Varlık Arama istemci kitaplığıyla varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [Node.jsiçin BING VARLıK arama SDK](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

Bing Varlık Arama SDK 'sını yüklemek için:

1. `npm install ms-rest-azure`Geliştirme ortamınızda çalıştırın.
2. `npm install @azure/cognitiveservices-entitysearch`Geliştirme ortamınızda çalıştırın.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ekleyin.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. `CognitiveServicesCredentials`Abonelik anahtarınızı kullanarak bir örnek oluşturun. Ardından, bununla birlikte arama istemcisinin bir örneğini oluşturun.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>İstek gönderme ve yanıt alma

1. İle bir varlık arama isteği gönderin `entitiesOperations.search()` . Bir yanıt aldıktan sonra, `queryContext` döndürülen sonuç sayısını ve ilk sonucun açıklamasını yazdırın.

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