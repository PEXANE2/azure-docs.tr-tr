---
title: 'Quickstart: C için Bing Yazım Denetimi SDK ile yazım denetimi #'
titleSuffix: Azure Cognitive Services
description: Yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API'sini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: aahi
ms.openlocfilehash: 1cda7032d5bfe58e9f8bcbdb8b18dd597a691441
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78273524"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-sdk-for-c"></a>Quickstart: C için Bing Yazım Denetimi SDK ile yazım denetimi #

C# için Bing Yazım Denetimi SDK ile yazım denetimine başlamak için bu hızlı başlangıcı kullanın. Bing Yazım Denetimi çoğu programlama diliyle uyumlu bir REST API'sine sahip olsa da, SDK hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/SpellCheck)bulunabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

* [Visual Studio 2017 veya sonrası](https://visualstudio.microsoft.com/downloads/)herhangi bir baskı .
* Bing Yazım Denetimi [NuGet Paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck)

Projenize Bing Büyüsü Denetimi SDK'sını eklemek için Visual Studio'daki **Solution Explorer'dan** **NuGet Paketlerini Yönet'i** seçin. `Microsoft.Azure.CognitiveServices.Language.SpellCheck` paketini ekleyin. Paket ayrıca aşağıdaki bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol çözümü oluşturun. Ardından aşağıdaki `using` ifadeyi ekleyin.
    
    ```csharp
    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck.Models;
    ```

2. Yeni bir sınıf oluşturun. Ardından, abonelik anahtarı nı `SpellCheckCorrection()` alan ve yazım denetimi isteğini gönderen bir eşzamanlı işlev oluşturun.

3. Yeni `ApiKeyServiceClientCredentials` bir nesne oluşturarak istemciyi anında anons edin. 

    ```csharp
    public static class SpellCheckSample{
        public static async Task SpellCheckCorrection(string key){
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials(key));
        }
        //...
    }
    ```

## <a name="send-the-request-and-read-the-response"></a>İsteği gönderin ve yanıtı okuyun

1. Yukarıda oluşturulan işlevde aşağıdaki adımları gerçekleştirin. Yazım denetimi isteğini istemciye gönderin. `text` Denetlenecek metni parametreye ekleyin ve modu `proof`'da ayarlayın  
    
    ```csharp
    var result = await client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof");
    ```

2. Varsa, ilk yazım denetimi sonucunu alın. Döndürülen ilk yanlış yazılmış sözcüğü (belirteç), belirteç türünü ve öneri sayısını yazdırın.

    ```csharp
    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();
    
    if (firstspellCheckResult != null)
    {
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);
    }
    ```

3. Varsa, önerilen ilk düzeltmeyi alın. Öneri puanını ve önerilen sözcüğü yazdırın. 

    ```csharp
    var suggestions = firstspellCheckResult.Suggestions;

    if (suggestions?.Count > 0)
    {
        var firstSuggestion = suggestions.FirstOrDefault();
        Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
        Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
    }
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi oluşturun ve çalıştırın. Visual Studio kullanıyorsanız, dosyayı hata ayıklamak için **F5** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorials/spellcheck.md)

- [Bing Yazım Denetimi API’si nedir?](overview.md)
- [Bing Yazım Denetimi C# SDK başvuru kılavuzu](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingspellcheck?view=azure-dotnet)
