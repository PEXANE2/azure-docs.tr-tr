---
title: Bing Video Arama C# istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: d50e1acd104916d68f7fbb84ff568cf4efc0b46b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80289759"
---
C# için Bing Video Arama istemci kitaplığı ile haber aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Aramaçoğu programlama diliyle uyumlu bir REST API'ye sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örneğin kaynak kodu, ek ek açıklamalar ve özelliklerle [GitHub'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch) bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017 veya sonrası](https://visualstudio.microsoft.com/downloads/)herhangi bir baskı .
* Json.NET çerçevesi, [NuGet paketi olarak](https://www.nuget.org/packages/Newtonsoft.Json/)kullanılabilir.

Bing Video Arama istemci kitaplığını projenize eklemek için Visual Studio'daki **Solution Explorer'dan** **NuGet Paketlerini Yönet'i** seçin. `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paketini ekleyin.

[[NuGet Video Search SDK paketinin]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) yüklenmesi de aşağıdaki bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol çözümü oluşturun. Ardından ana kod dosyasına aşağıdakileri ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;
    ```

2. Abonelik anahtarınızla yeni `ApiKeyServiceClientCredentials` bir nesne oluşturarak ve oluşturucuyu çağırarak istemciyi anında anons edin.

    ```csharp
    var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

## <a name="send-a-search-request-and-process-the-results"></a>Arama isteği gönderme ve sonuçları işleme

1. Arama isteği göndermek için istemciyi kullanın. Arama sorgusu için "SwiftKey"i kullanın.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
    ```

2. Herhangi bir sonuç döndürüldüyse, `videoResults.Value[0]`ilk ini alın. Ardından videonun kimliğini, başlığını ve url'sini yazdırın.

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
> [Tek bir sayfaweb uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../../overview.md)
* [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
