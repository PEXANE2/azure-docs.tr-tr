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
ms.date: 07/30/2019
ms.author: assafi
ms.openlocfilehash: 6bd3907392dad626c1eeb1823c929f1a35d544dd
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697665"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Hızlı Başlangıç: .NET SDK ve C# metin analizi hizmetini çağırmak için kullanın
<a name="HOLTop"></a>

Bu hızlı başlangıç, .NET için Azure SDK 'Yı kullanmaya ve C# dili çözümlemeye başlamanıza yardımcı olur. [Metin Analizi](//go.microsoft.com/fwlink/?LinkID=759711) REST API çoğu programlama dili ile uyumlu olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar.

> [!NOTE]
> Bu makaledeki tanıtım kodu, basitlik için Metin Analizi .NET SDK 'sının zaman uyumlu yöntemlerini kullanır. Ancak, üretim senaryolarında, kendi uygulamalarınızda toplanmış zaman uyumsuz yöntemleri kullanarak ölçeklenebilir ve hızlı yanıt vermeye devam etmenizi öneririz. Örneğin, `SentimentBatchAsync` `Sentiment`yerine kullanabilirsiniz.
>
> Bu örneğin toplanmış bir zaman uyumsuz sürümü [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics)' da bulunabilir.

Teknik Ayrıntılar için, .NET [metin analizi Reference](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet)SDK 'sına bakın.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir Visual Studio 2017 veya üzeri sürümü
* [.NET için metin analizi SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

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
        SentimentAnalysisExample(client);
        // DetectLanguageExample(client);
        // RecognizeEntitiesExample(client);
        // KeyPhraseExtractionExample(client);
        Console.ReadLine();
    }
```

Aşağıdaki bölümlerde her bir hizmet özelliğinin nasıl çağrılacağını açıklamaktadır.

## <a name="perform-sentiment-analysis"></a>Yaklaşım analizini gerçekleştirme

1. Daha önce oluşturduğunuz istemciyi `SentimentAnalysisExample()` alan yeni bir işlev oluşturun.
2. Aynı işlevde, sonucunu çağırın `client.Sentiment()` ve elde edin. Sonuç, başarılı `Score` olursa bu yaklaşım ve bir `errorMessage` ise, değildir. 0 ' a yakın olan bir puan negatif bir yaklaşımı gösterir, 1 ' e yakın bir puan pozitif bir yaklaşımı gösterir.

    ```csharp
    var result = client.Sentiment("I had the best day of my life.", "en");

    // Printing sentiment results
    Console.WriteLine($"Sentiment Score: {result.Score:0.00}");
    ```

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Dil algılamayı gerçekleştir

1. Daha önce oluşturduğunuz istemciyi `DetectLanguageExample()` alan yeni bir işlev oluşturun.
2. Aynı işlevde, sonucunu çağırın `client.DetectLanguage()` ve elde edin. Sonuç, başarılı olursa ' de `DetectedLanguages` algılanan dillerin listesini içerir `errorMessage` ve bu, değildir. Ardından ilk döndürülen dili yazdırın.

    ```csharp
    var result = client.DetectLanguage("This is a document written in English.");

    // Printing detected languages
    Console.WriteLine($"Language: {result.DetectedLanguages[0].Name}");
    ```

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. `countryHint` Parametresini 2 harfli bir ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""` olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz.

### <a name="output"></a>Output

```console
Language: English
```

## <a name="perform-entity-recognition"></a>Varlık tanıma gerçekleştir

1. Daha önce oluşturduğunuz istemciyi `RecognizeEntitiesExample()` alan yeni bir işlev oluşturun.
2. Aynı işlevde, sonucunu çağırın `client.Entities()` ve elde edin. Sonra sonuçlar arasında yineleme yapın. Sonuç, başarılı olursa ' de `Entities` algılanan varlıkların listesini içerir `errorMessage` ve bunu değildir. Algılanan her varlık için, türünü, alt türünü, Viseı adını (varsa) ve özgün metindeki konumları yazdırın.

    ```csharp
    var result = client.Entities("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.");

    // Printing recognized entities
    Console.WriteLine("Entities:");
    foreach (var entity in result.Entities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
        }
    }
    ```

### <a name="output"></a>Output

```console
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
2. Aynı işlevde, sonucunu çağırın `client.KeyPhrases()` ve elde edin. Sonuç, başarılı olursa ' de `KeyPhrases` algılanan anahtar tümceciklerin listesini içerir `errorMessage` ve bunu değildir. Ardından algılanan anahtar tümceciklerini yazdırın.

    ```csharp
    var result = client.KeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in result.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
    ```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)


* [Metin Analizine genel bakış](../overview.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
