---
title: Bing Özel Arama C# istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: ba80d1396b30b61bdfe4c121220429f5a7d994b0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79485990"
---
C# için Bing Özel Arama istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Bing Özel Arama API'si, önem verdiğiniz konular için özel, reklamsız arama deneyimleri oluşturmanıza olanak tanır. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch)bulunabilir.

Aşağıdakiler için C# için Bing Özel Arama istemci kitaplığını kullanın:
* Bing Özel Arama örneğinden web'de arama sonuçlarını bulun.

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingcustomsearch?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)


## <a name="prerequisites"></a>Ön koşullar

- Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](../../quick-start.md)
- Microsoft [.NET Core](https://www.microsoft.com/net/download/core)
- [Visual Studio 2017 veya sonrası](https://www.visualstudio.com/downloads/) herhangi bir sürümü
- Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
- [Bing Özel Arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) NuGet paketi. 
    - Visual **Studio'daki Solution Explorer'dan** projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet'i** seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin. NuGet Özel Arama paketini yüklediğinizde aşağıdaki derlemeler de yüklenir:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun. Ardından projenize aşağıdaki paketleri ekleyin.

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
    ```

2. Uygulamanızın ana yönteminde, arama istemcisini API anahtarınızla anında anınız edin.

    ```csharp
    var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));
    ```

## <a name="send-the-search-request-and-receive-a-response"></a>Arama isteğini gönderin ve yanıt alın
    
1. İstemcinizin `SearchAsync()` yöntemini kullanarak bir arama sorgusu gönderin ve yanıtı kaydedin. Örneğinizin `YOUR-CUSTOM-CONFIG-ID` yapılandırma kimliğiyle değiştirdiğinizden emin olun (Kimliği Bing [Özel Arama portalında](https://www.customsearch.ai/)bulabilirsiniz). Bu örnekte "Xbox" aranıyor.

    ```csharp
    // This will look up a single query (Xbox).
    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
    ```

2. `SearchAsync()` metodu bir `WebData` nesnesi döndürür. Nesneyi, bulunan herhangi bir `WebPages` nesne aracılığıyla yinelemek için kullanın. Bu kod ilk web sayfası sonucunu bulur ve web sayfasının `Name` ile `URL` değerlerini yazdırır.

    ```csharp
    if (webData?.WebPages?.Value?.Count > 0)
    {
        // find the first web page
        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

        if (firstWebPagesResult != null)
        {
            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
            Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
            Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
        }
        else
        {
            Console.WriteLine("Couldn't find web results!");
        }
    }
    else
    {
        Console.WriteLine("Didn't see any Web data..");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Arama web uygulaması oluşturma](../../tutorials/custom-search-web-page.md)
