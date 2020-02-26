---
title: 'Hızlı başlangıç: Metin Analizi istemci kitaplığı C# | Microsoft Docs'
description: İçin Metin Analizi istemci kitaplığı ile çalışmaya başlamaC#
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/15/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: b95f7fd14239b258fe1b501fdf71cf0dfe2beacf
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590707"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

v3 [başvurusu belgeleri](https://aka.ms/azsdk-net-textanalytics-ref-docs) | v3 [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3 paketi (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3 örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

v2 [başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [v2 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2 paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/)

[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Visual Studio IDE 'yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu, tek C# kaynak dosya içeren bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan Paket Yöneticisi ' nde **, Seç '** i seçin, **ön sürümü dahil**et ' i işaretleyin ve `Azure.AI.TextAnalytics`arayın. Sürüm `1.0.0-preview.2`' yi seçin ve sonra ' yi **yükler**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

```csharp
using System;
using Azure.AI.TextAnalytics;
```

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly TextAnalyticsApiKeyCredential credentials = new TextAnalyticsApiKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Uygulamanın `Main` yöntemini değiştirin. Burada, daha sonra çağrılan yöntemleri tanımlayacaksınız.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan paket yöneticisinde, **Araştır** ' ı seçin ve `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`arayın. Üzerine tıklayın ve sonra **uygulamasını**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Uygulamanın `Main` yöntemini değiştirin. Burada, daha sonra çağrılan yöntemleri tanımlayacaksınız.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemci, anahtarınızı kullanarak Azure 'da kimlik doğrulayan ve tek dizeler veya toplu işlem olarak metin kabul etmek için işlevler sağlayan bir `TextAnalyticsClient` nesnesidir. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. 

Sürüm `3.0-preview`kullanıyorsanız, istemciyi çeşitli varsayılan ayarlarla başlatmak için isteğe bağlı bir `TextAnalyticsClientOptions` örneğini kullanabilirsiniz (örneğin, varsayılan dil veya ülke ipucu). Ayrıca Azure Active Directory belirteci kullanarak kimlik doğrulaması yapabilirsiniz. 

## <a name="code-examples"></a>Kod örnekleri

* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner)
* [Kişisel bilgileri Algıla](#detect-personal-information)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Daha önce gelen ana yönteminizin bitiş noktanız ve kimlik bilgilerinizle yeni bir istemci nesnesi oluşturduğundan emin olun.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Kimlik bilgilerini depolamak ve istemcinin isteklerine eklemek için yeni bir `ApiKeyServiceClientCredentials` sınıfı oluşturun. İçinde, `Ocp-Apim-Subscription-Key` üstbilgisine anahtarınızı ekleyen `ProcessHttpRequestAsync()` için bir geçersiz kılma oluşturun.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Uç noktanız ve anahtarınızı içeren bir `ApiKeyServiceClientCredentials` nesnesi ile [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesini başlatmak için bir yöntem oluşturun.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Daha önce oluşturduğunuz istemciyi alan `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun ve `AnalyzeSentiment()` işlevini çağırın. Döndürülen `Response<DocumentSentiment>` nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, Ayrıca başarılı olursa her tümce için de yaklaşım analizini içerir. Bir hata oluşursa, bir `RequestFailedException`oluşturur.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    var si = new StringInfo(inputText);
    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tSentence [offset {sentence.Offset}, length {sentence.Length}]");
        Console.WriteLine($"\tText: \"{si.SubstringByTextElements(sentence.Offset, sentence.Length)}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.SentimentScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.SentimentScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.SentimentScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Document sentiment: Positive

        Sentence [offset 0, length 30]
        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [offset 31, length 30]
        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturduğunuz istemciyi alan `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun ve [Sentiment ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) işlevini çağırın. Döndürülen [Sentimentresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) nesnesi başarılı olursa yaklaşım `Score` ve değilse bir `errorMessage` içerecektir. 

0 ' a yakın olan bir puan negatif bir yaklaşımı gösterir, 1 ' e yakın bir puan pozitif bir yaklaşımı gösterir.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)


Daha önce oluşturduğunuz istemciyi alan `LanguageDetectionExample()` adlı yeni bir işlev oluşturun ve `DetectLanguage()` işlevini çağırın. Döndürülen `Response<DetectedLanguage>` nesnesi, algılanan dilin adı ve ISO-6391 koduyla birlikte içerecektir. Bir hata oluşursa, bir `RequestFailedException`oluşturur.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""`olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak için `TextAnalyticsClientOptions.DefaultCountryHint` özelliğini ayarlayın ve istemcinin başlatılması sırasında geçirin.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturduğunuz istemciyi alan `languageDetectionExample()` adlı yeni bir işlev oluşturun ve bunun [DetectLanguage ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Döndürülen [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) nesnesi, başarılı olursa `DetectedLanguages` algılanan dillerin listesini ve değilse bir `errorMessage` içerecektir. İlk döndürülen dili yazdır.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""` olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Çıktı

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)


> [!NOTE]
> Sürüm `3.0-preview`yenilikleri:
> * Varlık tanıma artık metindeki kişisel bilgileri algılama özelliğini içerir.
> * Varlık bağlama artık varlık tanıma işleminden ayrılmıştır.


Daha önce oluşturduğunuz istemciyi alan `EntityRecognitionExample()` adlı yeni bir işlev oluşturun, `RecognizeEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesnesi algılanan varlıkların listesini içerecektir. Bir hata oluşursa, bir `RequestFailedException`oluşturur.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach(var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Named Entities:
        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Offset: 26,     Length: 7,      Score: 0.920

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Offset: 34,     Length: 9,      Score: 0.800
```

## <a name="detect-personal-information"></a>Kişisel bilgileri Algıla

Daha önce oluşturduğunuz istemciyi alan `EntityPIIExample()` adlı yeni bir işlev oluşturun, `RecognizePiiEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Önceki işleve benzer şekilde, döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesnesi algılanan varlıkların listesini içerecektir. Bir hata oluşursa, bir `RequestFailedException`oluşturur.

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    string inputText = "Insurance policy for SSN on file 123-12-1234 is here by approved.";
    var response = client.RecognizePiiEntities(inputText);
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach(var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Category: U.S. Social Security Number (SSN),    Sub-Category: None
                Offset: 33,     Length: 11,     Score: 0.850
```


## <a name="entity-linking"></a>Varlık bağlama

Daha önce oluşturduğunuz istemciyi alan `EntityLinkingExample()` adlı yeni bir işlev oluşturun, `RecognizeLinkedEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<IReadOnlyCollection<LinkedEntity>>` algılanan varlıkların listesini temsil eder. Bir hata oluşursa, bir `RequestFailedException`oluşturur. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları bir `LinkedEntity` nesnesi altında `LinkedEntityMatch` nesnelerin listesi olarak gruplandırılır.

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
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.Id},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.Score:F3}\n");
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
                Offset: 116,    Length: 11,     Score: 0.777

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25,     Length: 10,     Score: 0.555

                Text: Gates
                Offset: 161,    Length: 5,      Score: 0.555

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40,     Length: 10,     Score: 0.533

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0,      Length: 9,      Score: 0.469

                Text: Microsoft
                Offset: 150,    Length: 9,      Score: 0.469

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54,     Length: 7,      Score: 0.248

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89,     Length: 5,      Score: 0.281
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

> [!NOTE]
> Sürüm 2,1 ' de, varlık bağlama, NER yanıtına dahil edilir.

Daha önce oluşturduğunuz istemciyi alan `RecognizeEntitiesExample()` adlı yeni bir işlev oluşturun ve [varlıklarını ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Sonuçlar arasında yineleme yapın. Döndürülen [Entitiesresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) nesnesi, başarılı olursa `Entities` algılanan varlıkların listesini ve değilse bir `errorMessage` içerir. Algılanan her varlık için, türünü, alt türünü, Viseı adını (varsa) ve özgün metindeki konumları yazdırın.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Daha önce oluşturduğunuz istemciyi alan `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun ve `ExtractKeyPhrases()` işlevini çağırın. Döndürülen `<Response<IReadOnlyCollection<string>>` nesnesi, algılanan anahtar tümceciklerin listesini içerecektir. Bir hata oluşursa, bir `RequestFailedException`oluşturur.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturduğunuz istemciyi alan `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun ve [KeyPhrases ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Sonuç, başarılı olursa `KeyPhrases` algılanan anahtar tümceciklerin listesini ve değilse bir `errorMessage` içerir. Algılanan tüm anahtar tümceleri yazdır.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```

---
