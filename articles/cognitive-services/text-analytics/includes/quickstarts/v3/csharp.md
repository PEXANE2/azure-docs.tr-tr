---
title: 'Hızlı başlangıç: Metin Analizi v3 istemci kitaplığı C# | Microsoft Docs'
description: İçin v3 Metin Analizi istemci kitaplığı ile çalışmaya başlamaC#
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/15/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 2cf9a006e1f6f1edb996aa9beaf8934a14af29df
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281210"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [paketi (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

> [!NOTE]
> * Bu hızlı başlangıçta, iyileştirilmiş [yaklaşım Analizi](../../../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../../../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel bir önizleme içeren Metin Analizi istemci kitaplığının sürüm `3.0-preview` kullanılmaktadır.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Örneğin, `AnalyzeSentiment()`yerine `AnalyzeSentimentAsync()` çağrılıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/)

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Visual Studio IDE 'yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu, tek C# kaynak dosya içeren bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan Paket Yöneticisi ' nde **, Seç '** i seçin, **ön sürümü dahil**et ' i işaretleyin ve `Azure.AI.TextAnalytics`arayın. Üzerine tıklayın ve sonra **uygulamasını**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

```csharp
using System;
using Azure.AI.TextAnalytics;
```

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Uygulamanın `Main` yöntemini değiştirin. Burada, daha sonra çağrılan yöntemleri tanımlayacaksınız.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, key);

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

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemci, anahtarınızı kullanarak Azure 'da kimlik doğrulayan ve tek dizeler veya toplu işlem olarak metin kabul etmek için işlevler sağlayan bir `TextAnalyticsClient` nesnesidir. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. İsteğe bağlı bir `TextAnalyticsClientOptions` örneği, istemciyi çeşitli varsayılan ayarlarla başlatmak için kullanılabilir (örneğin, varsayılan dil veya ülke ipucu).

AAD kimlik doğrulaması ve istemci varsayılan ayarlarının kullanımı dahil ek örnekler [burada](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)bulunabilir.

## <a name="code-examples"></a>Kod örnekleri

* Yaklaşım [Analizi](#sentiment-analysis) (Genel Önizleme)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-public-preview) (Genel Önizleme)
* [Adlandırılmış varlık tanıma-kişisel bilgiler](#named-entity-recognition---personal-information-public-preview) (Genel Önizleme)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

Programınızın `main()` yönteminde, istemcinin örneğini oluşturmak için kimlik doğrulama yöntemini çağırın.

## <a name="sentiment-analysis-public-preview"></a>Yaklaşım Analizi (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan Yaklaşım Analizi v3 içindir.

Daha önce oluşturduğunuz istemciyi alan `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun ve `AnalyzeSentiment()` işlevini çağırın. Döndürülen `Response<AnalyzeSentimentResult>` nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, başarılı olursa her tümce için de bir yaklaşım analizini ve değilse bir `Value.ErrorMessage` içerir.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    var response = client.AnalyzeSentiment("I had the best day of my life. I wish you were there with me.");
    Console.WriteLine($"Document sentiment: {response.Value.DocumentSentiment.SentimentClass}\n");
    foreach (var sentence in response.Value.SentenceSentiments)
    {
        Console.WriteLine($"\tSentence [offset {sentence.Offset}, length {sentence.Length}]");
        Console.WriteLine($"\tSentence sentiment: {sentence.SentimentClass}");
        Console.WriteLine($"\tPositive score: {sentence.PositiveScore:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.NegativeScore:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.NeutralScore:0.00}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Document sentiment: Positive

        Sentence [offset 0, length 30]
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [offset 31, length 30]
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

## <a name="language-detection"></a>Dil algılama

Daha önce oluşturduğunuz istemciyi alan `LanguageDetectionExample()` adlı yeni bir işlev oluşturun ve `DetectLanguage()` işlevini çağırın. Döndürülen `Response<DetectLanguageResult>` nesnesi, başarılı olursa `Value.PrimaryLanguage` algılanan dili ve değilse `Value.ErrorMessage` içerecektir.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""`olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak için `TextAnalyticsClientOptions.DefaultCountryHint` özelliğini ayarlayın ve istemcinin başlatılması sırasında geçirin.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    var response = client.DetectLanguage("Ce document est rédigé en Français.");
    var detectedLanguage = response.Value.PrimaryLanguage;
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Çıktı

```console
Language:
        French, ISO-6391: fr
```

## <a name="named-entity-recognition-public-preview"></a>Adlandırılmış varlık tanıma (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 içindir.

Daha önce oluşturduğunuz istemciyi alan `EntityRecognitionExample()` adlı yeni bir işlev oluşturun, `RecognizeEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<RecognizeEntitiesResult>` nesnesi, başarılı olursa `Value.NamedEntities` algılanan varlıkların listesini ve değilse `Value.ErrorMessage` içerir. Algılanan her varlık için, türü ve alt türü varsa yazdırın.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach(var entity in response.Value.NamedEntities)
    {
        Console.WriteLine($"\tText: {entity.Text},\tType: {entity.Type},\tSub-Type: {entity.SubType ?? "N/A"}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Named Entities:
        Text: Seattle,  Type: Location, Sub-Type: N/A
                Offset: 26,     Length: 7,      Score: 0.806

        Text: last week,        Type: DateTime, Sub-Type: N/A
                Offset: 34,     Length: 9,      Score: 0.800
```

## <a name="named-entity-recognition---personal-information-public-preview"></a>Adlandırılmış varlık tanıma-kişisel bilgiler (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 kullanan kişisel bilgileri tespit etmek içindir.

Daha önce oluşturduğunuz istemciyi alan `EntityPIIExample()` adlı yeni bir işlev oluşturun, `RecognizePiiEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Önceki işleve benzer şekilde, döndürülen `Response<RecognizeEntitiesResult>` nesnesi başarılı olursa `Value.NamedEntities` algılanan varlıkların listesini ve değilse `Value.ErrorMessage` içerir.

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    var response = client.RecognizePiiEntities("Insurance policy for SSN on file 123-12-1234 is here by approved.");
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach(var entity in response.Value.NamedEntities)
    {
        Console.WriteLine($"\tText: {entity.Text},\tType: {entity.Type},\tSub-Type: {entity.SubType ?? "N/A"}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Çıktı

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Type: U.S. Social Security Number (SSN),        Sub-Type: N/A
                Offset: 33,     Length: 11,     Score: 0.850
```

## <a name="entity-linking"></a>Varlık Bağlama

Daha önce oluşturduğunuz istemciyi alan `EntityLinkingExample()` adlı yeni bir işlev oluşturun, `RecognizeLinkedEntities()` işlevini çağırın ve sonuçlar arasında yineleme yapın. Döndürülen `Response<RecognizeLinkedEntitiesResult>` nesnesi, başarılı olursa `Value.LinkedEntities` algılanan varlıkların listesini ve değilse `Value.ErrorMessage` içerir. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları bir `LinkedEntity` nesnesi altında `LinkedEntityMatch` nesnelerin listesi olarak gruplandırılır.

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
    foreach (var entity in response.Value.LinkedEntities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.Id},\tURL: {entity.Uri}\tData Source: {entity.DataSource}");
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

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Daha önce oluşturduğunuz istemciyi alan `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun ve `ExtractKeyPhrases()` işlevini çağırın. Sonuç, başarılı olursa `Value.KeyPhrases` algılanan anahtar tümceciklerin listesini ve değilse bir `Value.ErrorMessage` içerir. Algılanan tüm anahtar tümceleri yazdır.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value.KeyPhrases)
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
