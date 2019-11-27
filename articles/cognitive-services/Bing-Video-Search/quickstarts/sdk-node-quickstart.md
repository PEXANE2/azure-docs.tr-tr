---
title: "Hızlı başlangıç: node. js için SDK 'Yı kullanarak videoları arama-Bing Video Arama"
titleSuffix: Azure Cognitive Services
description: Node. js için Bing Video Arama SDK 'sını kullanarak video arama istekleri göndermek için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 07/18/2019
ms.author: aahi
ms.openlocfilehash: 5c8bd4ccadcc3c1947905e6bd74b48045a62ab57
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383732"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Hızlı başlangıç: node. js için Bing Video Arama SDK ile video araması gerçekleştirme

Node. js için Bing Video Arama SDK 'Sı ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js)' da bulunabilir. Daha fazla ek açıklama ve özellik içerir.

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://www.nodejs.org/)

Bing Video Arama SDK kullanarak bir konsol uygulaması ayarlamak için:
* Geliştirme ortamınızda `npm install ms-rest-azure` çalıştırın.
* Geliştirme ortamınızda `npm install azure-cognitiveservices-videosearch` çalıştırın.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve Bing Video Arama SDK ve `CognitiveServicesCredentials` modülü için `require()` bir ifade ekleyin. Abonelik anahtarınız için bir değişken oluşturun. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Anahtarınızla `CognitiveServicesCredentials` bir örnek oluşturun. Daha sonra video arama istemcisinin bir örneğini oluşturmak için bunu kullanın.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Arama isteğini gönder

1. Bing Video Arama API'si bir arama isteği göndermek için `client.videosOperations.search()` kullanın. Arama sonuçları döndürüldüğünde, sonucu günlüğe kaydetmek için `.then()` kullanın.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı Web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)