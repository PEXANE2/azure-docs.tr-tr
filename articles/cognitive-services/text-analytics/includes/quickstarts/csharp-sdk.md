---
title: 'Quickstart: C# için Text Analytics istemci kitaplığı | Microsoft Dokümanlar'
description: 'C için Text Analytics istemci kitaplığı ile başlayın #'
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/17/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 2fa2e40ba2a7fe84b6df57bfb711d01332b8f523
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275364"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

[v3 Referans dokümantasyon](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [v3 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3 Paketi (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3 Örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[v2 Referans dokümantasyon](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [v2 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2 Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2 Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Görsel Stüdyo IDE](https://visualstudio.microsoft.com/vs/)
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun.  Dağıtıladıktan sonra **kaynağa git'i**tıklatın.
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Anahtarınızı ve bitiş noktanızı daha sonra hızlı başlatmada aşağıdaki koda yapıştıracaksınız.
    * Hizmeti denemek için ücretsiz`F0`fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Visual Studio IDE'yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu tek bir C # kaynak dosyası ile bir "Hello World" proje oluşturacak: *program.cs*.

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

**Çözüm Gezgini'ndeki** çözüme sağ tıklayarak ve **NuGet Paketlerini Yönet'i**seçerek istemci kitaplığını yükleyin. Seçili **Gözat'ı**açan paket yöneticisinde, ön `Azure.AI.TextAnalytics` **yayına ekle**seçeneğini işaretleyin ve '' yı arayın. Sürümü `1.0.0-preview.4`seçin ve sonra **yükleyin.** [Paket Yöneticisi Konsolu'nu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

**Çözüm Gezgini'ndeki** çözüme sağ tıklayarak ve **NuGet Paketlerini Yönet'i**seçerek istemci kitaplığını yükleyin. Açılan paket yöneticisinde **Gözat'ı** seçin `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`ve 'yi arayın. Üzerine tıklayın ve sonra **yükleyin.** [Paket Yöneticisi Konsolu'nu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs)bulabilirsiniz. 

---

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

*program.cs* dosyasını açın ve `using` aşağıdaki yönergeleri ekleyin:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve bitiş noktası için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Uygulama yöntemini `Main` değiştirin. Burada çağrılan yöntemleri daha sonra tanımlarsınız.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

*program.cs* dosyasını açın ve `using` aşağıdaki yönergeleri ekleyin:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve bitiş noktası için değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Uygulama yöntemini `Main` değiştirin. Burada çağrılan yöntemleri daha sonra tanımlarsınız.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>Nesne modeli

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan ve metni tek dizeleri veya toplu iş olarak kabul eden işlevler sağlayan bir `TextAnalyticsClient` nesnedir. API'ye eşzamanlı veya eşzamanlı olarak metin gönderebilirsiniz. Yanıt nesnesi, gönderdiğiniz her belgenin çözümleme bilgilerini içerir. 

Hizmetin sürümünü `3.0-preview` kullanıyorsanız, istemciyi çeşitli varsayılan `TextAnalyticsClientOptions` ayarlarla (örneğin varsayılan dil veya ülke ipucu) başlatmayı isteğe bağlı bir örnek kullanabilirsiniz. Azure Etkin Dizin belirteci kullanarak da kimlik doğrulaması yapabilirsiniz. 

## <a name="code-examples"></a>Kod örnekleri

* [Duygusallık analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık Tanıma](#named-entity-recognition-ner)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümcecik çıkarma](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Önceki ana yönteminizin bitiş noktanız ve kimlik bilgilerinizle yeni bir istemci nesnesi oluşturduğundan emin olun.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Kimlik bilgilerini `ApiKeyServiceClientCredentials` depolamak ve istemcinin isteklerine eklemek için yeni bir sınıf oluşturun. İçinde, anahtarınızı `ProcessHttpRequestAsync()` `Ocp-Apim-Subscription-Key` üstbilgiye ekleyen bir geçersiz kılma oluşturun.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

[TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesini bitiş noktanızla ve anahtarınızı içeren `ApiKeyServiceClientCredentials` bir nesneyle anında oluşturmak için bir yöntem oluşturun.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Daha önce oluşturduğunuz istemciyi alan ve `SentimentAnalysisExample()` `AnalyzeSentiment()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `Response<DocumentSentiment>` nesne, tüm giriş belgesinin duyarlılık etiketini ve puanını ve başarılı olursa her cümle için bir duygu analizini içerir. Bir hata varsa, bir `RequestFailedException`.

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

Daha önce oluşturduğunuz istemciyi alan ve onun `SentimentAnalysisExample()` [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) işlevini çağıran yeni bir işlev oluşturun. Döndürülen [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) nesnesi `Score` başarılı olursa `errorMessage` duygu yu içerecektir ve değilse. 

0'a yakın bir skor olumsuz bir duyguyu gösterirken, 1'e yakın bir skor olumlu bir duyguyu gösterir.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)


Daha önce oluşturduğunuz istemciyi alan ve `LanguageDetectionExample()` `DetectLanguage()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `Response<DetectedLanguage>` nesne, adı ve ISO-6391 koduyla birlikte algılanan dili içerir. Bir hata varsa, bir `RequestFailedException`.

> [!Tip]
> Bazı durumlarda girişe göre dilleri ayrıştırmak zor olabilir. Parametreyi `countryHint` 2 harfli ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak API varsayılan countryHint olarak "ABD" kullanıyor, bu davranışı kaldırmak için bu değeri boş `countryHint = ""`dize için ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak `TextAnalyticsClientOptions.DefaultCountryHint` için özelliği ayarlayın ve istemcinin başlatma sırasında geçirin.

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

Daha önce oluşturduğunuz istemciyi alan yeni `languageDetectionExample()` bir işlev oluşturun ve [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Döndürülen [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) nesnesi, başarılı `DetectedLanguages` olursa algılanan dillerin listesini ve `errorMessage` başarısız olursa listesini içerir. Döndürülen ilk dili yazdırın.

> [!Tip]
> Bazı durumlarda girişe göre dilleri ayrıştırmak zor olabilir. Parametreyi `countryHint` 2 harfli ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak API varsayılan countryHint olarak "ABD" kullanıyor, bu davranışı kaldırmak için bu değeri boş `countryHint = ""` dize için ayarlayarak bu parametreyi sıfırlayabilirsiniz.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Çıktı

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)


> [!NOTE]
> Sürümde `3.0-preview`yeni:
> * Varlık bağlantısı artık varlık tanımadan ayrılmış bir durumdur.


Daha önce oluşturduğunuz istemciyi alan, `EntityRecognitionExample()` `RecognizeEntities()` işlevini çağıran ve sonuçları yineleyen yeni bir işlev oluşturun. Döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesne, algılanan varlıkların listesini içerir. Bir hata varsa, bir `RequestFailedException`.

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

## <a name="entity-linking"></a>Varlık bağlama

Daha önce oluşturduğunuz istemciyi alan, `EntityLinkingExample()` `RecognizeLinkedEntities()` işlevini çağıran ve sonuçları yineleyen yeni bir işlev oluşturun. Döndürülen `Response<IReadOnlyCollection<LinkedEntity>>` varlıklar, algılanan varlıkların listesini temsil eder. Bir hata varsa, bir `RequestFailedException`. Bağlı varlıklar benzersiz olarak tanımlandığından, aynı varlığın oluşumları `LinkedEntity` `LinkedEntityMatch` nesnelerin listesi olarak bir nesne altında gruplandırılır.

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
> Sürüm 2.1'de, varlık bağlantısı NER yanıtına dahildir.

Daha önce oluşturduğunuz istemciyi alan ve `RecognizeEntitiesExample()` [Varlıklarını()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran yeni bir işlev oluşturun. Sonuçları yineleyin. Döndürülen [VarlıklarSonuç](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) nesnesi, başarılı `Entities` olursa algılanan varlıkların listesini `errorMessage` ve başarısız olursa listesini içerir. Algılanan her varlık için, özgün metindeki konumları (varsa) Türünü, Alt Türünü, Vikipedi adını yazdırın.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Daha önce oluşturduğunuz istemciyi alan ve `KeyPhraseExtractionExample()` `ExtractKeyPhrases()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `<Response<IReadOnlyCollection<string>>` nesne, algılanan anahtar tümceciklerin listesini içerir. Bir hata varsa, bir `RequestFailedException`.

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

Daha önce oluşturduğunuz istemciyi alan ve `KeyPhraseExtractionExample()` Anahtar [Tümcecikleri()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran yeni bir işlev oluşturun. Sonuç, başarılı `KeyPhrases` olursa algılanan anahtar tümceciklerin listesini `errorMessage` ve başarısız olursa listesini içerir. Algılanan anahtar tümcecikleri yazdırın.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```

---
