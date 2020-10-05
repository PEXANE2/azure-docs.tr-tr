---
title: Bing Video Arama JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: fa1ecb24649d9355e6104bff8add5430fdd64c3c
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91377199"
---
JavaScript için Bing Video Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js)' da bulunabilir. Daha fazla ek açıklama ve özellik içerir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.
* [JavaScript için Bing video arama SDK 'sı](https://www.npmjs.com/package/@azure/cognitiveservices-videosearch)
     *  Yüklemek için şunu çalıştırın `npm install @azure/cognitiveservices-videosearch`
* `CognitiveServicesCredentials` `@azure/ms-rest-azure-js` İstemcinin kimliğini doğrulamak için paketteki sınıf.
     * Yüklemek için şunu çalıştırın `npm install @azure/ms-rest-azure-js`

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve `require()` Bing video arama istemci kitaplığı ve modülü için bir ifade ekleyin `CognitiveServicesCredentials` . Abonelik anahtarınız için bir değişken oluşturun. 
    
    ```javascript
    const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('@azure/cognitiveservices-videosearch');
    ```

2. Anahtarınızla bir örnek oluşturun `CognitiveServicesCredentials` . Daha sonra video arama istemcisinin bir örneğini oluşturmak için bunu kullanın.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Arama isteğini gönder

1. `client.videosOperations.search()`Bing video arama API'si bir arama isteği göndermek için kullanın. Arama sonuçları döndürüldüğünde, `.then()` sonucu günlüğe kaydetmek için kullanın.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı Web uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../../overview.md)
* [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)