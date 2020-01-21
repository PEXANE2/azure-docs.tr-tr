---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/28/2019
ms.author: aahi
ms.openlocfilehash: de6601a60bbb65787673d9d3bf5b1cbe9f46804a
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281386"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

> [!NOTE]
> Bu makaledeki kod, basitlik için Metin Analizi .NET SDK 'sının zaman uyumlu yöntemlerini kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Örneğin, [Sentiment ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet)yerine [Sentimentbatchasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentimentbatchasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet-preview) çağrılıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/)

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Visual Studio IDE 'yi kullanarak yeni bir .NET Core konsol uygulaması oluşturun. Bu, tek C# bir kaynak dosyası olan basit bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

**Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklemelisiniz. Açılan paket yöneticisinde, **Araştır** ' ı seçin ve `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`arayın. Üzerine tıklayın ve sonra **uygulamasını**. [Paket Yöneticisi konsolunu](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)da kullanabilirsiniz.

*Program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Uygulamanın `Program` sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info-v2.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Uygulamanın `Main` yöntemini değiştirin. Burada, daha sonra çağrılan yöntemleri tanımlayacaksınız.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemci, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesidir ve metni tek dizeler veya toplu işlem olarak kabul etmek için işlevler sağlar. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Varlık tanıma](#entity-recognition)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Kimlik bilgilerini depolamak ve istemcinin isteklerine eklemek için yeni bir `ApiKeyServiceClientCredentials` sınıfı oluşturun. İçinde, `Ocp-Apim-Subscription-Key` üstbilgisine anahtarınızı ekleyen `ProcessHttpRequestAsync()` için bir geçersiz kılma oluşturun.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Uç noktanız ve anahtarınızı içeren bir `ApiKeyServiceClientCredentials` nesnesi ile [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) nesnesini başlatmak için bir yöntem oluşturun.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

Programınızın `main()` yönteminde, istemcinin örneğini oluşturmak için kimlik doğrulama yöntemini çağırın.

## <a name="sentiment-analysis"></a>Duygu analizi

Daha önce oluşturduğunuz istemciyi alan `SentimentAnalysisExample()` adlı yeni bir işlev oluşturun ve [Sentiment ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) işlevini çağırın. Döndürülen [Sentimentresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) nesnesi başarılı olursa yaklaşım `Score` ve değilse bir `errorMessage` içerecektir. 

0 ' a yakın olan bir puan negatif bir yaklaşımı gösterir, 1 ' e yakın bir puan pozitif bir yaklaşımı gösterir.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

### <a name="output"></a>Çıktı

```console
Sentiment Score: 0.87
```

## <a name="language-detection"></a>Dil algılama

Daha önce oluşturduğunuz istemciyi alan `languageDetectionExample()` adlı yeni bir işlev oluşturun ve bunun [DetectLanguage ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Döndürülen [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) nesnesi, başarılı olursa `DetectedLanguages` algılanan dillerin listesini ve değilse bir `errorMessage` içerecektir.  İlk döndürülen dili yazdır.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""` olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Çıktı

```console
Language: English
```

## <a name="entity-recognition"></a>Varlık tanıma

Daha önce oluşturduğunuz istemciyi alan `RecognizeEntitiesExample()` adlı yeni bir işlev oluşturun ve [varlıklarını ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Sonuçlar arasında yineleme yapın. Döndürülen [Entitiesresult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) nesnesi, başarılı olursa `Entities` algılanan varlıkların listesini ve değilse bir `errorMessage` içerir. Algılanan her varlık için, türünü, alt türünü, Viseı adını (varsa) ve özgün metindeki konumları yazdırın.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]


### <a name="output"></a>Çıktı

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

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Daha önce oluşturduğunuz istemciyi alan `KeyPhraseExtractionExample()` adlı yeni bir işlev oluşturun ve [KeyPhrases ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) işlevini çağırın. Sonuç, başarılı olursa `KeyPhrases` algılanan anahtar tümceciklerin listesini ve değilse bir `errorMessage` içerir. Algılanan tüm anahtar tümceleri yazdır.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Çıktı

```console
Key phrases:
    cat
    veterinarian
```
