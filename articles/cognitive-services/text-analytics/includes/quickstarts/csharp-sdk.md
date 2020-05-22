---
title: 'Hızlı başlangıç: C# için Metin Analizi istemci kitaplığı | Microsoft Docs'
description: 'C için Metin Analizi istemci kitaplığı ile çalışmaya başlama #'
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/17/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 0e98a10573a2e3abda255c325845190ed5067bb3
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83778275"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

[v3 başvuru belgeleri](https://aka.ms/azsdk-net-textanalytics-ref-docs)  |  [v3 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics)  |  [V3 paketi (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics)  |  [v3 örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[v2 başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview)  |  [v2 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics)  |  [v2 paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/)  |  [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/)
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir metin analizi kaynağı oluşturun metin analizi bir kaynak oluşturun.  Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Visual Studio IDE 'yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu, tek bir C# kaynak dosyası içeren bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan paket yöneticisinde, Seç ' i seçin, **ön sürümü dahil** **et ' i**işaretleyin ve arama yapın `Azure.AI.TextAnalytics` . Sürüm `1.0.0-preview.4` ' ü ve ardından **öğesini seçin**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan paket yöneticisinde, **Araştır** ' ı seçin ve arama yapın `Microsoft.Azure.CognitiveServices.Language.TextAnalytics` . Üzerine tıklayın ve sonra **uygulamasını**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs)bulabilirsiniz. 

---

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
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
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

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

Metin Analizi istemcisi, `TextAnalyticsClient` anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir nesnedir ve metni tek dizeler veya toplu işlem olarak kabul etmek için işlevler sağlar. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. 

Hizmetin sürümünü kullanıyorsanız `3.0-preview` , `TextAnalyticsClientOptions` istemciyi çeşitli varsayılan ayarlarla başlatmak için isteğe bağlı bir örnek kullanabilirsiniz (örneğin, varsayılan dil veya ülke/bölge ipucu). Ayrıca Azure Active Directory belirteci kullanarak kimlik doğrulaması yapabilirsiniz. 

## <a name="code-examples"></a>Kod örnekleri

* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık Tanıma](#named-entity-recognition-ner)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Daha önce gelen ana yönteminizin bitiş noktanız ve kimlik bilgilerinizle yeni bir istemci nesnesi oluşturduğundan emin olun.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

`ApiKeyServiceClientCredentials`Kimlik bilgilerini depolamak ve istemcinin isteklerine eklemek için yeni bir sınıf oluşturun. Bunun içinde anahtarınızı üstbilgiye ekleyen için bir geçersiz kılma oluşturun `ProcessHttpRequestAsync()` `Ocp-Apim-Subscription-Key` .

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Uç noktanız ve anahtarınızı içeren bir nesne ile [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesini başlatmak için bir yöntem oluşturun `ApiKeyServiceClientCredentials` .

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`SentimentAnalysisExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `AnalyzeSentiment()` işlevini çağırın. Döndürülen `Response<DocumentSentiment>` nesne, tüm giriş belgesi için yaklaşım etiketini ve Puanını, başarılı olursa her tümce için de bir yaklaşım analizini içerir. Bir hata oluşursa, bir oluşturur `RequestFailedException` .

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

`SentimentAnalysisExample()`Daha önce oluşturduğunuz istemciyi alan çağrılan ve [Sentiment ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) işlevini çağıran yeni bir işlev oluşturun. Döndürülen [Sentimentresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) nesnesi `Score` başarılı olursa yaklaşım ve bir if ifadesi içerir `errorMessage` . 

0 ' a yakın olan bir puan negatif bir yaklaşımı gösterir, 1 ' e yakın bir puan pozitif bir yaklaşımı gösterir.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)


`LanguageDetectionExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `DetectLanguage()` işlevini çağırın. Döndürülen `Response<DetectedLanguage>` nesne, adı ve ISO-6391 koduyla birlikte algılanan dili içerir. Bir hata oluşursa, bir oluşturur `RequestFailedException` .

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. `countryHint`Parametresini 2 harfli bir ülke/bölge kodu belirtmek için kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz `countryHint = ""` . Farklı bir varsayılan ayarlamak için, özelliği ayarlayın `TextAnalyticsClientOptions.DefaultCountryHint` ve istemcinin başlatılması sırasında geçirin.

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

`languageDetectionExample()`Daha önce oluşturduğunuz istemciyi alıp [DetectLanguage ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran adlı yeni bir işlev oluşturun. Döndürülen [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) nesnesi, başarılı olursa ' de algılanan dillerin listesini içerir `DetectedLanguages` ve bunu `errorMessage` değildir. İlk döndürülen dili yazdır.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. `countryHint`Parametresini 2 harfli bir ülke/bölge kodu belirtmek için kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz `countryHint = ""` .

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Çıktı

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)


> [!NOTE]
> Sürümünde yeni `3.0-preview` :
> * Varlık bağlama artık varlık tanıma işleminden ayrılmıştır.


`EntityRecognitionExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun, `RecognizeEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<IReadOnlyCollection<CategorizedEntity>>` nesne, algılanan varlıkların listesini içerir. Bir hata oluşursa, bir oluşturur `RequestFailedException` .

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

`EntityLinkingExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun, `RecognizeLinkedEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<IReadOnlyCollection<LinkedEntity>>` varlıkların listesini temsil eder. Bir hata oluşursa, bir oluşturur `RequestFailedException` . Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları `LinkedEntity` nesne listesi olarak bir nesne altında gruplandırılır `LinkedEntityMatch` .

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

> [!NOTE]
> Sürüm 2,1 ' de, varlık bağlama, NER yanıtına dahil edilir.

`RecognizeEntitiesExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve [varlıklarını ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Sonuçlar arasında yineleme yapın. Döndürülen [Entitiesresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) nesnesi, başarılı olursa ' de algılanan varlıkların listesini içerir `Entities` ve bunu `errorMessage` değildir. Algılanan her varlık için, türünü, alt türünü, Viseı adını (varsa) ve özgün metindeki konumları yazdırın.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`KeyPhraseExtractionExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `ExtractKeyPhrases()` işlevini çağırın. Döndürülen `<Response<IReadOnlyCollection<string>>` nesne, algılanan anahtar tümceciklerin listesini içerir. Bir hata oluşursa, bir oluşturur `RequestFailedException` .

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

`KeyPhraseExtractionExample()`Daha önce oluşturduğunuz istemciyi alıp [KeyPhrases ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağıran adlı yeni bir işlev oluşturun. Sonuç, başarılı olursa ' de algılanan anahtar tümceciklerin listesini içerir `KeyPhrases` ve bunu `errorMessage` değildir. Algılanan tüm anahtar tümceleri yazdır.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```

---
