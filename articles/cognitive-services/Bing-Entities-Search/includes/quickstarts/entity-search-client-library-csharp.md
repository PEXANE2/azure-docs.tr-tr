---
title: Bing Entity Search C# istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 39a6c21ad056980e8c7b146e36a6e185cb3ed95e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79136784"
---
C# için Bing Entity Search istemci kitaplığı olan varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Entity Search çoğu programlama diliyle uyumlu bir REST API'sine sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch)bulunabilir.


## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017 veya sonrası](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
* [Bing Haberler Arama SDK NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Bu paketin yüklenmesi de aşağıdakileri yükler:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Görsel Stüdyo projenize Bing Varlık Arama istemci kitaplığını eklemek için **Solution Explorer'dan** `Microsoft.Azure.CognitiveServices.Search.EntitySearch` **NuGet Paketlerini Yönet** seçeneğini kullanın ve paketi ekleyin.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Bir uygulama oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol çözümü oluşturun. Ardından ana kod dosyasına aşağıdakileri ekleyin.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>İstemci oluşturma ve arama isteği gönderme

1. Yeni bir arama istemcisi oluşturun. Yeni `ApiKeyServiceClientCredentials`bir .

    ```csharp
    var client = new EntitySearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. Sorgunuzu aramak `Entities.Search()` için istemcinin işlevini kullanın:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Varlık açıklamasını alma ve yazdırma

1. API arama sonuçlarını döndürürse, `entityData`ana varlığı ' dan'

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Ana varlığın açıklamasını yazdırma 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md )