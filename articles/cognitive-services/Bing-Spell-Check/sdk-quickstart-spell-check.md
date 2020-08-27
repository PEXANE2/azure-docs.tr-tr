---
title: 'Hızlı başlangıç: C için Bing Yazım Denetimi SDK ile yazım denetimi #'
titleSuffix: Azure Cognitive Services
description: Yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: e1e05ca9b63bda3373afb8a090118953d89ad8f9
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934234"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-sdk-for-c"></a>Hızlı başlangıç: C için Bing Yazım Denetimi SDK ile yazım denetimi #

C# için Bing Yazım Denetimi SDK ile yazım denetimi başlatmak için bu hızlı başlangıcı kullanın. Bing Yazım Denetimi birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/SpellCheck)' da bulunabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

* Herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/downloads/)sürümü.
* Bing Yazım Denetimi [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck)

Projenize Bing Yazım Denetimi SDK eklemek için, Visual Studio 'da Çözüm Gezgini **NuGet Paketlerini Yönet** ' **Solution Explorer** i seçin. `Microsoft.Azure.CognitiveServices.Language.SpellCheck` paketini ekleyin. Paket de aşağıdaki bağımlılıkları da yüklüyor:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio 'da yeni bir C# konsol çözümü oluşturun. Ardından aşağıdaki ifadeyi ekleyin `using` .
    
    ```csharp
    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck.Models;
    ```

2. Yeni bir sınıf oluşturun. Ardından `SpellCheckCorrection()` , bir abonelik anahtarı alan ve yazım denetimi isteğini gönderen adlı bir zaman uyumsuz işlev oluşturun.

3. Yeni bir nesne oluşturarak istemciyi örneğini oluşturun `ApiKeyServiceClientCredentials` . 

    ```csharp
    public static class SpellCheckSample{
        public static async Task SpellCheckCorrection(string key){
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials(key));
        }
        //...
    }
    ```

## <a name="send-the-request-and-read-the-response"></a>İsteği gönder ve yanıtı oku

1. Yukarıda oluşturulan işlevde aşağıdaki adımları gerçekleştirin. Yazım denetimi isteğini istemcisiyle gönderin. Parametreye denetlenecek metni ekleyin `text` ve modunu olarak ayarlayın `proof` .  
    
    ```csharp
    var result = await client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof");
    ```

2. Varsa, ilk yazım denetimi sonucunu alın. Döndürülen ilk yanlış sözcüğü (belirteci), belirteç türünü ve öneri sayısını yazdır.

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

3. Bir varsa, önerilen ilk düzeltmeyi alın. Öneri Puanını ve önerilen sözcüğü yazdırın. 

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

Projenizi derleyin ve çalıştırın. Visual Studio kullanıyorsanız, dosyada hata ayıklamak için **F5** ' e basın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorials/spellcheck.md)

- [Bing Yazım Denetimi API’si nedir?](overview.md)
- [Bing Yazım Denetimi C# SDK başvuru kılavuzu](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingspellcheck?view=azure-dotnet)
