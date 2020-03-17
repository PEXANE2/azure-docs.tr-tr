---
title: 'Hızlı Başlangıç: C# için Metin Analizi istemci kitaplığı | Microsoft Docs'
description: C# için Metin Analizi istemci kitaplığını kullanmaya başlayın
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 6adce0ed6b5b5768bd9a489fced25ce439a33e0a
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79203446"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

[v3 Başvuru belgeleri](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [v3 Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3 Paketi (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3 Örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[v2 Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [v2 Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2 Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2 Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz olarak oluşturun](https://azure.microsoft.com/free/)
* [Visual Studio IDE](https://visualstudio.microsoft.com/vs/)
* Azure aboneliğinizi oluşturduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portalda <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Metin Analizi kaynağı oluşturun"  target="_blank">Metin Analizi kaynağı oluşturun <span class="docon docon-navigate-external x-hidden-focus"></span></a>. 
    * Uygulamanızı Metin Analizi API’sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu hızlı başlangıçta daha sonra yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-net-core-application"></a>Yeni .NET Core uygulaması oluşturma

Visual Studio IDE’yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu, tek bir C# kaynak dosyasıyla (*program.cs*) “Merhaba Dünya” adında bir proje oluşturur.

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

**Çözüm Gezgini**’nde çözüme sağ tıklayarak ve **NuGet paketlerini yönet** seçeneğini belirleyerek istemci kitaplığını yükleyin. Açılan paket yöneticisinde **Gözat**’ı seçip **Ön sürümü dahil et** seçeneğini işaretleyin ve `Azure.AI.TextAnalytics` için arama yapın. Sürüm olarak `1.0.0-preview.2` seçin ve **Yükle** seçeneğini belirleyin. [Paket Yöneticisi Konsolu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)’nu da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodunu aynı anda görüntülemek mi istiyorsunuz? Bunu, bu hızlı başlangıçtaki kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs)’da bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

**Çözüm Gezgini**’nde çözüme sağ tıklayarak ve **NuGet paketlerini yönet** seçeneğini belirleyerek istemci kitaplığını yükleyin. Açılan paket yöneticisinde **Gözat**’ı seçin ve `Microsoft.Azure.CognitiveServices.Language.TextAnalytics` için arama yapın. Üzerine tıklayın ve **Yükle** seçeneğini belirleyin. [Paket Yöneticisi Konsolu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)’nu da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodunu aynı anda görüntülemek mi istiyorsunuz? Bunu, bu hızlı başlangıçtaki kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs)’da bulabilirsiniz. 

---

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergesini ekleyin:

```csharp
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Uygulamanın `Program` sınıfında kaynağınızın anahtarına ve uç noktasına yönelik değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly TextAnalyticsApiKeyCredential credentials = new TextAnalyticsApiKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Uygulamanın `Main` yöntemini değiştirin. Burada çağrılan yöntemleri daha sonra tanımlayacaksınız.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityPIIExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergesini ekleyin:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Uygulamanın `Program` sınıfında kaynağınızın anahtarına ve uç noktasına yönelik değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Uygulamanın `Main` yöntemini değiştirin. Burada çağrılan yöntemleri daha sonra tanımlayacaksınız.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak Azure’da kimlik doğrulaması yapan ve metnin tek dizeler halinde veya bir toplu iş olarak kabul edilmesine olanak tanıyan işlevleri sunan bir `TextAnalyticsClient` nesnesidir. Metinleri API’ye zaman uyumlu veya zaman uyumsuz olarak gönderebilirsiniz. Yanıt nesnesi, gönderdiğiniz her belgeye yönelik analiz bilgilerini içerir. 

`3.0-preview` sürümünü kullanıyorsanız istemciyi çeşitli varsayılan ayarlarla başlatmak için isteğe bağlı bir `TextAnalyticsClientOptions` örneği kullanabilirsiniz (örneğin varsayılan dil veya ülke ipucu). Bir Azure Active Directory belirteci kullanarak da kimlik doğrulaması yapabilirsiniz. 

## <a name="code-examples"></a>Kod örnekleri

* [Yaklaşım analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık Tanıma](#named-entity-recognition-ner)
* [Kişisel bilgileri algılama](#detect-personal-information)
* [Varlık bağlama](#entity-linking)
* [Anahtar ifade ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Daha önce kullandığınız ana yöntemin uç noktanız ve kimlik bilgilerinizle yeni bir istemci nesnesi oluşturduğundan emin olun.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Kimlik bilgilerini depolamak ve bunları istemcinin isteklerine eklemek için yeni bir `ApiKeyServiceClientCredentials` sınıfı oluşturun. Bu sınıfta, anahtarınızı `Ocp-Apim-Subscription-Key` üst bilgisine ekleyen `ProcessHttpRequestAsync()` için bir geçersiz kılma işlevi oluşturun.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Uç noktanızda [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesinin ve anahtarınızı içeren `ApiKeyServiceClientCredentials` nesnesinin örneğini oluşturmak için bir yöntem oluşturun.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Önceden oluşturduğunuz istemciyi alan ve bunun `AnalyzeSentiment()` işlevini çağıran `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun. Döndürülen `Response<DocumentSentiment>` nesnesi yaklaşım etiketini, tüm giriş belgesinin puanını ve başarılı olması durumunda her cümleye yönelik yaklaşım analizini içerir. Hata oluşursa bir `RequestFailedException` oluşturulur.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    var si = new StringInfo(inputText);
    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tSentence [length {sentence.GraphemeLength}]");
        Console.WriteLine($"\tText: \"{si.SubstringByTextElements(sentence.GraphemeOffset, sentence.GraphemeLength)}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Document sentiment: Positive

        Sentence [length 30]
        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [length 30]
        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Önceden oluşturduğunuz istemciyi alan ve bunun [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) işlevini çağıran `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun. Döndürülen [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) nesnesi başarılı olursa `Score` yaklaşımını içerir. Nesne başarısız olursa bir `errorMessage` içerir. 

0’a yakın puanlar negatif yaklaşımı, 1’e yakın puanlar ise pozitif yaklaşımı gösterir.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)


Önceden oluşturduğunuz istemciyi alan ve bunun `DetectLanguage()` işlevini çağıran `LanguageDetectionExample()` adlı yeni bir işlev oluşturun. Döndürülen `Response<DetectedLanguage>` nesnesi, adıyla ve ISO-6391 koduyla birlikte algılanan dili içerir. Hata oluşursa bir `RequestFailedException` oluşturulur.

> [!Tip]
> Bazı durumlarda girişi temel alarak dilleri netleştirmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. API, countryHint için varsayılan olarak “US”değerini kullanır. Bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""` olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak için `TextAnalyticsClientOptions.DefaultCountryHint` özelliğini ayarlayın ve müşterinin başlatma işlemi esnasında bunu geçirin.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Çıktı

```console
Language:
        French, ISO-6391: fr
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Önceden oluşturduğunuz istemciyi alan ve bunun [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran `languageDetectionExample()` adlı yeni bir işlev oluşturun. Döndürülen [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) nesnesi başarılı olursa `DetectedLanguages` öğesinde algılanan dillerin listesini içerir. Nesne başarısız olursa bir `errorMessage` içerir. İlk döndürülen dili yazdırın.

> [!Tip]
> Bazı durumlarda girişi temel alarak dilleri netleştirmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. API, countryHint için varsayılan olarak “US”değerini kullanır. Bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""` olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Çıktı

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)


> [!NOTE]
> `3.0-preview` sürümündeki yenilikler:
> * Varlık algılama artık metindeki kişisel bilgileri algılama özelliğini içeriyor.
> * Varlık bağlama artık varlık tanımadan ayrıldı.


Önceden oluşturduğunuz istemciyi alan ve bunun `RecognizeEntities()` işlevini çağırıp sonuçları yineleyen `EntityRecognitionExample()` adlı yeni bir işlev oluşturun. Döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesnesi algılanan varlıkların listesini içerir. Hata oluşursa bir `RequestFailedException` oluşturulur.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tLength: {entity.GraphemeLength},\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Named Entities:
        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Length: 7,      Score: 0.92

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Length: 9,      Score: 0.80
```

## <a name="detect-personal-information"></a>Kişisel bilgileri algılama

Önceden oluşturduğunuz istemciyi alan ve bunun `RecognizePiiEntities()` işlevini çağırıp sonuçları yineleyen `EntityPIIExample()` adlı yeni bir işlev oluşturun. Önceki işleve benzer olarak, döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesnesi algılanan varlıkların listesini içerir. Hata oluşursa bir `RequestFailedException` oluşturulur.

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    string inputText = "Insurance policy for SSN on file 123-12-1234 is here by approved.";
    var response = client.RecognizePiiEntities(inputText);
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tLength: {entity.GraphemeLength},\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Category: U.S. Social Security Number (SSN),    Sub-Category:
                Length: 11,     Score: 0.85
```


## <a name="entity-linking"></a>Varlık bağlama

Önceden oluşturduğunuz istemciyi alan ve bunun `RecognizeLinkedEntities()` işlevini çağırıp sonuçları yineleyen `EntityLinkingExample()` adlı yeni bir işlev oluşturun. Döndürülen `Response<IReadOnlyCollection<LinkedEntity>>` değeri algılanan varlıkların listesini temsil eder. Hata oluşursa bir `RequestFailedException` oluşturulur. Bağlı varlıklar benzersiz olarak tanımlandıkları için aynı varlıkların oluşumu `LinkedEntityMatch` nesnelerinin listesi olarak bir `LinkedEntity` nesnesinde gruplandırılır.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tLength: {match.GraphemeLength},\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>Çıktı

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Length: 11,     Score: 0.78

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Length: 10,     Score: 0.55

                Text: Gates
                Length: 5,      Score: 0.55

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Length: 10,     Score: 0.53

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Length: 9,      Score: 0.47

                Text: Microsoft
                Length: 9,      Score: 0.47

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Length: 7,      Score: 0.25

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Length: 5,      Score: 0.28
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

> [!NOTE]
> Sürüm 2.1’de, varlık bağlama NER yanıtına dahil edilir.

Önceden oluşturduğunuz istemciyi alan ve bunun [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran `RecognizeEntitiesExample()` adlı yeni bir işlev oluşturun. Sonuçları yineleyin. Döndürülen [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) nesnesi başarılı olursa `Entities` öğesinde algılanan varlıkların listesini içerir. Nesne başarısız olursa bir `errorMessage` içerir. Algılanan her varlık için bu varlığa yönelik Tür, Alt tür, Wikipedia adı (varsa) ve asıl metindeki konumları yazdırın.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Önceden oluşturduğunuz istemciyi alan ve bunun `ExtractKeyPhrases()` işlevini çağıran `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun. Döndürülen `<Response<IReadOnlyCollection<string>>` nesnesi algılanan anahtar ifadelerin listesini içerir. Hata oluşursa bir `RequestFailedException` oluşturulur.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Önceden oluşturduğunuz istemciyi alan ve bunun [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun. Sonuç başarılı olursa `KeyPhrases` öğesindeki algılanan anahtar ifadelerin listesini içerir. Sonuç başarısız olursa bir `errorMessage` içerir. Algılanan tüm anahtar ifadeleri yazdırın.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```

---
