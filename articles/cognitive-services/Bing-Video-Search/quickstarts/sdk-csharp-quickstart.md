---
title: Hızlı başlangıç:-Bing Video Arama için C# SDK kullanarak video arama
titleSuffix: Azure Cognitive Services
description: İçin C#Bing video arama SDK 'sını kullanarak video arama istekleri göndermek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: 1cbb5880c38aca649dffb91f31b9340648b85f68
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75382523"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-c"></a>Hızlı başlangıç: için Bing Video Arama SDK ile bir video araması gerçekleştirinC#

İçin C#Bing video arama SDK 'sı ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu, [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch) 'da ek ek açıklamalar ve özelliklerle bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/downloads/)sürümü.
* [NuGet paketi olarak](https://www.nuget.org/packages/Newtonsoft.Json/)kullanılabilen JSON.NET Framework.

Projenize Bing Video Arama SDK eklemek için, Visual Studio 'da Çözüm Gezgini **NuGet Paketlerini Yönet** ' i seçin. `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paketini ekleyin.

[[NuGet VIDEO arama SDK paketi]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) yüklemesi, aşağıdaki bağımlılıkları da yüklüyor:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio 'da C# yeni bir konsol çözümü oluşturun. Ardından aşağıdakini ana kod dosyasına ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;
    ```

2. Abonelik anahtarınızla yeni bir `ApiKeyServiceClientCredentials` nesnesi oluşturup oluşturucuyu çağırarak istemciyi oluşturun.

    ```csharp
    var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

## <a name="send-a-search-request-and-process-the-results"></a>Arama isteği gönderme ve sonuçları işleme

1. Bir arama isteği göndermek için istemcisini kullanın. Arama sorgusu için "SwiftKey" kullanın.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
    ```

2. Herhangi bir sonuç döndürülürse, `videoResults.Value[0]`ilk birini alın. Ardından videonun KIMLIĞINI, başlığını ve URL 'sini yazdırın.

    ```csharp
    if (videoResults.Value.Count > 0)
    {
        var firstVideoResult = videoResults.Value[0];

        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
        Console.WriteLine($"First video name: {firstVideoResult.Name}");
        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
    }
    else
    {
        Console.WriteLine("Couldn't find video results!");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı Web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
