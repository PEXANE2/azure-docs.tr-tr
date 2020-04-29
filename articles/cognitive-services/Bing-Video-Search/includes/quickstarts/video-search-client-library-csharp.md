---
title: Bing Video Arama C# istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: d50e1acd104916d68f7fbb84ff568cf4efc0b46b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80289759"
---
C# için Bing Video Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu, [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch) 'da ek ek açıklamalar ve özelliklerle bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/downloads/)sürümü.
* [NuGet paketi olarak](https://www.nuget.org/packages/Newtonsoft.Json/)kullanılabilen JSON.NET Framework.

Bing Video Arama istemci kitaplığını projenize eklemek için, Visual Studio 'daki Çözüm Gezgini **NuGet Paketlerini Yönet** ' **Solution Explorer** i seçin. `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paketini ekleyin.

[[NuGet VIDEO arama SDK paketi]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) yüklemesi, aşağıdaki bağımlılıkları da yüklüyor:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio 'da yeni bir C# konsol çözümü oluşturun. Ardından aşağıdakini ana kod dosyasına ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;
    ```

2. Abonelik anahtarınızla yeni `ApiKeyServiceClientCredentials` bir nesne oluşturup oluşturucuyu çağırarak istemciyi örneğini oluşturun.

    ```csharp
    var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

## <a name="send-a-search-request-and-process-the-results"></a>Arama isteği gönderme ve sonuçları işleme

1. Bir arama isteği göndermek için istemcisini kullanın. Arama sorgusu için "SwiftKey" kullanın.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
    ```

2. Herhangi bir sonuç döndürülürse, ile `videoResults.Value[0]`birinciden yararlanın. Ardından videonun KIMLIĞINI, başlığını ve URL 'sini yazdırın.

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
> [Tek sayfalı Web uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../../overview.md)
* [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
