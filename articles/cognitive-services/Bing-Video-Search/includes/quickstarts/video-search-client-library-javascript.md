---
title: Bing Video Arama JavaScript istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: b642d981b79d13857881ec8cc5796e6365003ace
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80289766"
---
JavaScript için Bing Video Arama istemci kitaplığı ile haber aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Aramaçoğu programlama diliyle uyumlu bir REST API'ye sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js)bulunabilir. Daha fazla ek açıklama ve özellik içerir.

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://www.nodejs.org/)

Bing Video Arama istemci kitaplığını kullanarak bir konsol uygulaması ayarlamak için:
* Geliştirme `npm install ms-rest-azure` ortamınızda çalıştırın.
* Geliştirme `npm install azure-cognitiveservices-videosearch` ortamınızda çalıştırın.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve Bing `CognitiveServicesCredentials` Video Arama istemci kitaplığı ve modülü için bir `require()` deyim ekleyin. Abonelik anahtarınız için bir değişken oluşturun. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Anahtarınızla bir `CognitiveServicesCredentials` örnek oluşturun. Ardından, video arama istemcisinin bir örneğini oluşturmak için kullanın.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Arama isteğini gönderme

1. Bing `client.videosOperations.search()` Video Arama API'sine arama isteği göndermek için kullanın. Arama sonuçları döndürüldüğünde, `.then()` sonucu günlüğe kaydetmek için kullanın.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek bir sayfaweb uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../../overview.md)
* [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)