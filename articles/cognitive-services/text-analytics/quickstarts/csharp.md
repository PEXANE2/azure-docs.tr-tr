---
title: "Hızlı Başlangıç: .NET için Azure SDK 'sını kullanarak Metin Analizi hizmetini çağırın veC#"
titleSuffix: Azure Cognitive Services
description: Metin Analizi hizmetini ve ' i C#kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/18/2019
ms.author: assafi
ms.openlocfilehash: 09713528f51675f6e9d7f3073b6c81b095d23631
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68356968"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Hızlı Başlangıç: .NET SDK ve C# metin analizi hizmetini çağırmak için kullanın
<a name="HOLTop"></a>

Bu hızlı başlangıç, .NET için Azure SDK 'Yı kullanmaya ve C# dili çözümlemeye başlamanıza yardımcı olur. [Metin Analizi](//go.microsoft.com/fwlink/?LinkID=759711) REST API çoğu programlama dili ile uyumlu olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar.

> [!NOTE]
> Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics)’da mevcuttur.

Teknik Ayrıntılar için, .NET [metin analizi Reference](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet)SDK 'sına bakın.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir [Visual Studio 2017 veya üzeri] sürümü
* [.NET için metin analizi SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Kaydolma sırasında sizin için oluşturulan [uç nokta ve erişim anahtarına](../How-tos/text-analytics-how-to-access-key.md) de ihtiyacınız vardır.

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Visual Studio çözümünü oluşturma ve SDK 'Yı yüklemeye

1. Yeni bir konsol uygulaması (.NET Core) projesi oluşturun. [Visual Studio 'Ya erişin](https://visualstudio.microsoft.com/vs/).
1. Çözüme sağ tıklayın ve **çözüm Için NuGet Paketlerini Yönet**' i seçin.
1. **Gözat** sekmesini seçin. **Microsoft. Azure. Biliveservices. Language. TextAnalytics**için arama yapın.

## <a name="authenticate-your-credentials"></a>Kimlik bilgilerinizi doğrulama

1. Aşağıdaki `using` deyimlerini ana sınıf dosyasına ekleyin (varsayılan olarak program.cs olur).

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Kimlik bilgilerini depolamak `ApiKeyServiceClientCredentials` ve her istek için eklemek üzere yeni bir sınıf oluşturun.

    ```csharp
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string apiKey;

        public ApiKeyServiceClientCredentials(string apiKey)
        {
            this.apiKey = apiKey;
        }

        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }
            request.Headers.Add("Ocp-Apim-Subscription-Key", this.apiKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. `Program` Sınıfını güncelleştirin. Metin Analizi API'si anahtarınız için bir sabit üye ve hizmet uç noktası için bir tane ekleyin. Metin Analizi kaynağınız için doğru Azure konumunu kullanmayı unutmayın.

    ```csharp
    //Enter your Text Analytics (TA) API Key (available in Azure Portal -> your TA resource -> Keys)
    private const string ApiKey = "enter-your-textanalytics-api-key-here";
    //You can get the resource location from Azure Portal -> your TA resource -> Overview
    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://<your-location>.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Üretim sistemlerindeki gizli dizi güvenliğini artırmak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)kullanmanızı öneririz.
>

## <a name="create-a-text-analytics-client"></a>Metin Analizi istemcisi oluşturma

`Main` Projenizin işlevinde, çağırmak istediğiniz örnek yöntemi çağırın. `Endpoint` Tanımladığınız ve `ApiKey` parametrelerini geçirin.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(ApiKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

Aşağıdaki bölümlerde her bir hizmet özelliğinin nasıl çağrılacağını açıklamaktadır.

## <a name="perform-sentiment-analysis"></a>Yaklaşım analizini gerçekleştirme

1. Daha önce oluşturduğunuz istemciyi `SentimentAnalysisExample()` alan yeni bir işlev oluşturun.
2. Çözümlemek istediğiniz belgeleri içeren `MultiLanguageInput` nesnelerin bir listesini oluşturun.

    ```csharp
    public static async Task SentimentAnalysisExample(TextAnalyticsClient client)
    {
        // The documents to be analyzed. Add the language of the document. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "I had the best day of my life.")
            });
        //...
    }
    ```

3. Aynı işlevde, sonucunu çağırın `client.SentimentAsync()` ve elde edin. Sonra sonuçlar arasında yineleme yapın. Her belgenin KIMLIĞINI ve yaklaşım Puanını yazdır. 0 ' a yakın olan bir puan negatif bir yaklaşımı gösterir, 1 ' e yakın bir puan pozitif bir yaklaşımı gösterir.

    ```csharp
    var result = await client.SentimentAsync(false, inputDocuments);

    // Printing sentiment results
    foreach (var document in result.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Dil algılamayı gerçekleştir

1. Daha önce oluşturduğunuz istemciyi `DetectLanguageExample()` alan yeni bir işlev oluşturun.
2. Belgelerinizi içeren `LanguageInput` nesnelerin bir listesini oluşturun.

    ```csharp
    public static async Task DetectLanguageExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for language detection. The ID can be any value.
        var inputDocuments = new LanguageBatchInput(
                new List<LanguageInput>
                    {
                        new LanguageInput(id: "1", text: "This is a document written in English.")
                    });
        //...
    }
    ```

3. Aynı işlevde, sonucunu çağırın `client.DetectLanguageAsync()` ve elde edin. Sonra sonuçlar arasında yineleme yapın. Her belgenin KIMLIĞINI ve döndürülen ilk dili yazdır.

    ```csharp
    var langResults = await client.DetectLanguageAsync(false, inputDocuments);

    // Printing detected languages
    foreach (var document in langResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
    }
    ```

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
```

## <a name="perform-entity-recognition"></a>Varlık tanıma gerçekleştir

1. Daha önce oluşturduğunuz istemciyi `RecognizeEntitiesExample()` alan yeni bir işlev oluşturun.
2. Belgelerinizi içeren `MultiLanguageBatchInput` nesnelerin bir listesini oluşturun.

    ```csharp
    public static async Task RecognizeEntitiesExample(TextAnalyticsClient client)
    {
        // The documents to be submitted for entity recognition. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.")
            });
        //...
    }
    ```

3. Aynı işlevde, sonucunu çağırın `client.EntitiesAsync()` ve elde edin. Sonra sonuçlar arasında yineleme yapın. Her belgenin KIMLIĞINI yazdır. Algılanan her varlık için, bu dosyanın Visede adını ve türünü ve alt türlerini (varsa) ve özgün metindeki konumları yazdırın.

    ```csharp
    var entitiesResult = await client.EntitiesAsync(false, inputDocuments);

    // Printing recognized entities
    foreach (var document in entitiesResult.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Entities:");
        foreach (var entity in document.Entities)
        {
            Console.WriteLine($"\t\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
            foreach (var match in entity.Matches)
            {
                Console.WriteLine($"\t\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
            }
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 0,      Length: 9,      Score: 1.000
                Name: Bill Gates,       Type: Person,   Sub-Type: N/A
                        Offset: 25,     Length: 10,     Score: 1.000
                Name: Paul Allen,       Type: Person,   Sub-Type: N/A
                        Offset: 40,     Length: 10,     Score: 0.999
                Name: April 4,  Type: Other,    Sub-Type: N/A
                        Offset: 54,     Length: 7,      Score: 0.800
                Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
                        Offset: 54,     Length: 13,     Score: 0.800
                Name: BASIC,    Type: Other,    Sub-Type: N/A
                        Offset: 89,     Length: 5,      Score: 0.800
                Name: Altair 8800,      Type: Other,    Sub-Type: N/A
                        Offset: 116,    Length: 11,     Score: 0.800
```

## <a name="perform-key-phrase-extraction"></a>Anahtar tümceciği ayıklama gerçekleştir

1. Daha önce oluşturduğunuz istemciyi `KeyPhraseExtractionExample()` alan yeni bir işlev oluşturun.
2. Belgelerinizi içeren `MultiLanguageBatchInput` nesnelerin bir listesini oluşturun.

    ```csharp
    public static async Task KeyPhraseExtractionExample(TextAnalyticsClient client)
    {
        var inputDocuments = new MultiLanguageBatchInput(
                    new List<MultiLanguageInput>
                    {
                        new MultiLanguageInput("en", "1", "My cat might need to see a veterinarian.")
                    });
        //...
    }
    ```

3. Aynı işlevde, sonucunu çağırın `client.KeyPhrasesAsync()` ve elde edin. Sonra sonuçlar arasında yineleme yapın. Her belgenin KIMLIĞINI ve algılanan anahtar tümceleri yazdır.

    ```csharp
    var kpResults = await client.KeyPhrasesAsync(false, inputDocuments);

    // Printing keyphrases
    foreach (var document in kpResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Key phrases:");

        foreach (string keyphrase in document.KeyPhrases)
        {
            Console.WriteLine($"\t\t{keyphrase}");
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
         Key phrases:
                cat
                veterinarian
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)


* [Metin Analizine genel bakış](../overview.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
