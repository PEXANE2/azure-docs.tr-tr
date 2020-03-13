---
title: Bing Varlık Arama C# istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 39a6c21ad056980e8c7b146e36a6e185cb3ed95e
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136784"
---
İçin C#Bing varlık arama istemci kitaplığıyla varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch)' da bulunabilir.


## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir [Visual Studio 2017 veya üzeri](https://www.visualstudio.com/downloads/)sürümü.
* NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
* [BING haber arama SDK NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Bu paketin yüklenmesi aşağıdakileri de yapar:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Bing Varlık Arama istemci kitaplığını Visual Studio projenize eklemek için, **Çözüm Gezgini**' dan **NuGet Paketlerini Yönet** seçeneğini kullanın ve `Microsoft.Azure.CognitiveServices.Search.EntitySearch` paketini ekleyin.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Uygulama oluşturma ve başlatma

1. Visual Studio 'da C# yeni bir konsol çözümü oluşturun. Ardından aşağıdakini ana kod dosyasına ekleyin.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>İstemci oluşturma ve arama isteği gönderme

1. Yeni bir arama istemcisi oluşturun. Yeni bir `ApiKeyServiceClientCredentials`oluşturarak abonelik anahtarınızı ekleyin.

    ```csharp
    var client = new EntitySearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. Sorgunuzu aramak için istemcinin `Entities.Search()` işlevini kullanın:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Bir varlık açıklaması al ve Yazdır

1. API arama sonuçları döndürdüğünden `entityData`ana varlığı alın.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Ana varlığın açıklamasını yazdır 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md )