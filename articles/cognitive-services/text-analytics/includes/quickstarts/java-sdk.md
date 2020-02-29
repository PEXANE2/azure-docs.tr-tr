---
title: 'Hızlı başlangıç: Java için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Java için v3 Metin Analizi istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/26/2020
ms.author: aahi
ms.reviewer: tasharm, assafi
ms.openlocfilehash: 79b4063d6b65d6861dd7864c4225e91f4ea5bc6d
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155407"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://aka.ms/azsdk-java-textanalytics-ref-docs) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics) | [paketi (Maven)](https://oss.sonatype.org/#nexus-search;quick~com.azure) | [örnekleri](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK) sürüm 8 veya üzeri
* Azure aboneliğiniz olduktan sonra, anahtarınızı ve uç noktanızı almak için</a> Azure portal bir kaynak Metin Analizi <span class="docon docon-navigate-external x-hidden-focus"></span> oluşturun"  target="_blank">metin analizi bir kaynak oluşturun <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=". 
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu daha sonra hızlı başlangıçta yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-maven-project"></a>Yeni bir Maven projesi oluşturma

Projenize aşağıdaki metin analizi bağımlılığını ekleyin. Bağımlılığın bu sürümü Metin Analizi API'si `3.0-preview` sürümünü kullanır. 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.2</version>
    </dependency>
</dependencies>
```

Şu dizinde yeni bir Java dosyası oluşturun: `\src\main\java`.

Java dosyasını açın ve aşağıdaki `import` deyimlerini ekleyin:

```java
import com.azure.ai.textanalytics.models.*;
import com.azure.ai.textanalytics.TextAnalyticsClientBuilder;
import com.azure.ai.textanalytics.TextAnalyticsClient;
```

Java dosyasında, yeni bir sınıf ekleyin ve aşağıda gösterildiği gibi Azure kaynağınızın anahtarını ve uç noktasını ekleyin.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```java
public class TextAnalyticsSamples {
    private static String KEY = "<replace-with-your-text-analytics-key-here>";
    private static String ENDPOINT = "<replace-with-your-text-analytics-endpoint-here>";
}
```

Aşağıdaki Main yöntemini sınıfına ekleyin. Burada, daha sonra çağrılan yöntemleri tanımlayacaksınız.

```java
public static void main(String[] args) {

    TextAnalyticsClient client = authenticateClient(KEY, ENDPOINT);

    sentimentAnalysisExample(client);
    detectLanguageExample(client);
    recognizeEntitiesExample(client);
    recognizePIIEntitiesExample(client);
    recognizeLinkedEntitiesExample(client);
    extractKeyPhrasesExample(client);
}
```

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemci, anahtarınızı kullanarak Azure 'da kimlik doğrulayan ve tek dizeler veya toplu işlem olarak metin kabul etmek için işlevler sağlayan bir `TextAnalyticsClient` nesnesidir. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`KEY` ve yukarıda oluşturulan `ENDPOINT` `TextAnalyticsClient` nesnesini başlatmak için bir yöntem oluşturun.

```java
static TextAnalyticsClient authenticateClient(String key, String endpoint) {
    return new TextAnalyticsClientBuilder()
        .apiKey(new TextAnalyticsApiKeyCredential(key))
        .endpoint(endpoint)
        .buildClient();
}
```

Programınızın `main()` yönteminde, istemcinin örneğini oluşturmak için kimlik doğrulama yöntemini çağırın.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Daha önce oluşturduğunuz istemciyi alan `sentimentAnalysisExample()` adlı yeni bir işlev oluşturun ve `analyzeSentiment()` işlevini çağırın. Döndürülen `AnalyzeSentimentResult` nesnesi, `documentSentiment` ve `sentenceSentiments` başarılı olursa veya değilse `errorMessage` içerir. 

```java
static void sentimentAnalysisExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had the best day of my life. I wish you were there with me.";

    DocumentSentiment documentSentiment = client.analyzeSentiment(text);
        System.out.printf(
            "Recognized document sentiment: %s, positive score: %.2f, neutral score: %.2f, negative score: %.2f.%n",
            documentSentiment.getSentiment(),
            documentSentiment.getSentimentScores().getPositive(),
            documentSentiment.getSentimentScores().getNeutral(),
            documentSentiment.getSentimentScores().getNegative());

        for (SentenceSentiment sentenceSentiment : documentSentiment.getSentences()) {
            System.out.printf(
                "Recognized sentence sentiment: %s, positive score: %.2f, neutral score: %.2f, negative score: %.2f.%n",
                sentenceSentiment.getSentiment(),
                sentenceSentiment.getSentimentScores().getPositive(),
                sentenceSentiment.getSentimentScores().getNeutral(),
                sentenceSentiment.getSentimentScores().getNegative());
        }
}
```

### <a name="output"></a>Çıktı

```console
Recognized document sentiment: positive, Positive Score: 1.00, Neutral Score: 0.00, Negative Score: 0.00.
Recognized sentence sentiment: positive, positive score: 1.00, neutral score: 0.00, negative score: 0.00.
Recognized sentence sentiment: neutral, positive score: 0.21, neutral score: 0.77, negative score: 0.02.
```
## <a name="language-detection"></a>Dil algılama

Daha önce oluşturduğunuz istemciyi alan `detectLanguageExample()` adlı yeni bir işlev oluşturun ve `detectLanguage()` işlevini çağırın. Döndürülen `DetectLanguageResult` nesnesi, algılanan birincil dili, başarılı olursa algılanan diğer dillerin listesini veya değilse bir `errorMessage` içerir.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `countryHint = ""`olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak için `TextAnalyticsClientOptions.DefaultCountryHint` özelliğini ayarlayın ve istemcinin başlatılması sırasında geçirin.

```java
static void detectLanguageExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Ce document est rédigé en Français.";

    DetectedLanguage detectedLanguage = client.detectLanguage(text);
    System.out.printf("Detected primary language: %s, ISO 6391 name: %s, score: %.2f.%n",
        detectedLanguage.getName(),
        detectedLanguage.getIso6391Name(),
        detectedLanguage.getScore());
}
```

### <a name="output"></a>Çıktı

```console
Detected primary language: French, ISO 6391 name: fr, score: 1.00.
```
## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

> [!NOTE]
> Sürüm `3.0-preview`:
> * NER kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama, NER 'den ayrı bir istek.

Daha önce oluşturduğunuz istemciyi alan `recognizeEntitiesExample()` adlı yeni bir işlev oluşturun ve `recognizeEntities()` işlevini çağırın. Döndürülen `RecognizeEntitiesResult` nesnesi, başarılı olursa `NamedEntity` bir liste veya değilse `errorMessage` içerir.

```java
static void recognizeEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had a wonderful trip to Seattle last week.";

    for (CategorizedEntity entity : client.recognizeEntities(text)) {
        System.out.printf(
            "Recognized entity: %s, entity category: %s, entity sub-category: %s, offset: %s, length: %s, score: %.2f.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubCategory() == null || entity.getSubCategory().isEmpty() ? "N/A" : entity.getSubCategory(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized entity: Seattle, entity category: Location, entity sub-category: GPE, offset: 26, length: 7, score: 0.92.
Recognized entity: last week, entity category: DateTime, entity sub-category: DateRange, offset: 34, length: 9, score: 0.80.
```

## <a name="using-ner-to-recognize-personal-information"></a>Kişisel bilgileri tanımak için NER kullanma

Daha önce oluşturduğunuz istemciyi alan `recognizePIIEntitiesExample()` adlı yeni bir işlev oluşturun ve `recognizePiiEntities()` işlevini çağırın. Döndürülen `RecognizePiiEntitiesResult` nesnesi, başarılı olursa `NamedEntity` bir liste veya değilse `errorMessage` içerir. 

```java
static void recognizePIIEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Insurance policy for SSN on file 123-12-1234 is here by approved.";

    for (PiiEntity entity : client.recognizePiiEntities(text)) {
        System.out.printf(
            "Recognized personal identifiable information entity: %s, entity category: %s, entity sub-category: %s, offset: %s, length: %s, score: %.2f.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubCategory() == null || entity.getSubCategory().isEmpty() ? "N/A" : entity.getSubCategory(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized personal identifiable information entity: 123-12-1234, entity category: U.S. Social Security Number (SSN), entity sub-category: N/A, offset: 33, length: 11, score: 0.85.
```

## <a name="entity-linking"></a>Varlık bağlama

Daha önce oluşturduğunuz istemciyi alan `recognizeLinkedEntitiesExample()` adlı yeni bir işlev oluşturun ve `recognizeLinkedEntities()` işlevini çağırın. Döndürülen `RecognizeLinkedEntitiesResult` nesnesi, başarılı olursa `LinkedEntity` bir liste veya değilse `errorMessage` içerir. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları bir `LinkedEntity` nesnesi altında `LinkedEntityMatch` nesnelerin listesi olarak gruplandırılır.

```java
static void recognizeLinkedEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
            "to develop and sell BASIC interpreters for the Altair 8800. " +
            "During his career at Microsoft, Gates held the positions of chairman, " +
            "chief executive officer, president and chief software architect, " +
            "while also being the largest individual shareholder until May 2014.";

    System.out.printf("Linked Entities:%n");
    for (LinkedEntity linkedEntity : client.recognizeLinkedEntities(text)) {
        System.out.printf("Name: %s, ID: %s, URL: %s, Data Source: %s.%n",
                linkedEntity.getName(),
                linkedEntity.getId(),
                linkedEntity.getUrl(),
                linkedEntity.getDataSource());
        System.out.printf("Matches:%n");
        for (LinkedEntityMatch linkedEntityMatch : linkedEntity.getLinkedEntityMatches()) {
            System.out.printf("Text: %s, Offset: %s, Length: %s, Score: %.2f.%n",
                    linkedEntityMatch.getText(),
                    linkedEntityMatch.getOffset(),
                    linkedEntityMatch.getLength(),
                    linkedEntityMatch.getScore());
        }
    }
}
```

### <a name="output"></a>Çıktı

```console
Linked Entities:
Name: Altair 8800, ID: Altair 8800, URL: https://en.wikipedia.org/wiki/Altair_8800, Data Source: Wikipedia.
Matches:
Text: Altair 8800, Offset: 11, Length: 116, Score: 0.78.
Name: Bill Gates, ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates, Data Source: Wikipedia.
Matches:
Text: Bill Gates, Offset: 10, Length: 25, Score: 0.55.
Text: Gates, Offset: 5, Length: 161, Score: 0.55.
Name: Paul Allen, ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen, Data Source: Wikipedia.
Matches:
Text: Paul Allen, Offset: 10, Length: 40, Score: 0.53.
Name: Microsoft, ID: Microsoft, URL: https://en.wikipedia.org/wiki/Microsoft, Data Source: Wikipedia.
Matches:
Text: Microsoft, Offset: 9, Length: 0, Score: 0.47.
Text: Microsoft, Offset: 9, Length: 150, Score: 0.47.
Name: April 4, ID: April 4, URL: https://en.wikipedia.org/wiki/April_4, Data Source: Wikipedia.
Matches:
Text: April 4, Offset: 7, Length: 54, Score: 0.25.
Name: BASIC, ID: BASIC, URL: https://en.wikipedia.org/wiki/BASIC, Data Source: Wikipedia.
Matches:
Text: BASIC, Offset: 5, Length: 89, Score: 0.28.
```
## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

Daha önce oluşturduğunuz istemciyi alan `extractKeyPhrasesExample()` adlı yeni bir işlev oluşturun ve `extractKeyPhrases()` işlevini çağırın. Döndürülen `ExtractKeyPhraseResult` nesnesi, başarılı olursa anahtar tümceciklerin bir listesini ya da değilse bir `errorMessage` içerir.

```java
static void extractKeyPhrasesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "My cat might need to see a veterinarian.";

    System.out.printf("Recognized phrases: %n");
    for (String keyPhrase : client.extractKeyPhrases(text)) {
        System.out.printf("%s%n", keyPhrase);
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized phrases: 
cat
veterinarian
```
