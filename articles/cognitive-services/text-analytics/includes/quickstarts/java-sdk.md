---
title: 'Hızlı başlangıç: Java için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Java için v3 Metin Analizi istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/17/2020
ms.author: aahi
ms.reviewer: tasharm, assafi, sumeh
ms.openlocfilehash: bd070300427716634d786e685cfe1cf8e45a246c
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140775"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://aka.ms/azsdk-java-textanalytics-ref-docs)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics)  |  [Paket](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.4)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK) sürüm 8 veya üzeri
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir metin analizi kaynağı oluşturun metin analizi bir kaynak oluşturun.  Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="add-the-client-library"></a>İstemci kitaplığını ekleme

Tercih ettiğiniz IDE veya geliştirme ortamınızda Maven projesi oluşturun. Ardından projenizin *pom. xml* dosyasına aşağıdaki bağımlılığı ekleyin. [Diğer derleme araçlarının](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.4) uygulama sözdizimini çevrimiçi olarak bulabilirsiniz.

```xml
<dependencies>
     <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.5</version>
    </dependency>
</dependencies>
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/TextAnalytics/TextAnalyticsSamples.java)bulabilirsiniz. 

Adlı bir Java dosyası oluşturun `TextAnalyticsSamples.java` . Dosyasını açın ve aşağıdaki `import` deyimlerini ekleyin:

```java
import com.azure.core.credential.AzureKeyCredential;
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
    //You will create these methods later in the quickstart.
    TextAnalyticsClient client = authenticateClient(KEY, ENDPOINT);

    sentimentAnalysisExample(client);
    detectLanguageExample(client);
    recognizeEntitiesExample(client);
    recognizeLinkedEntitiesExample(client);
    extractKeyPhrasesExample(client);
}
```

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, `TextAnalyticsClient` anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir nesnedir ve metni tek dizeler veya toplu işlem olarak kabul etmek için işlevler sağlar. API 'ye eşzamanlı olarak veya zaman uyumsuz olarak metin gönderebilirsiniz. Yanıt nesnesi, göndereceğiniz her belge için analiz bilgilerini içerecektir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`TextAnalyticsClient`Metin analizi kaynağınız için anahtar ve uç nokta ile nesneyi örneklendirilecek bir yöntem oluşturun.

```java
static TextAnalyticsClient authenticateClient(String key, String endpoint) {
    return new TextAnalyticsClientBuilder()
        .credential(new AzureKeyCredential(key))
        .endpoint(endpoint)
        .buildClient();
}
```

Programınızın `main()` yönteminde, istemcisini başlatmak için kimlik doğrulama yöntemini çağırın.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

`sentimentAnalysisExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `analyzeSentiment()` işlevini çağırın. Döndürülen `AnalyzeSentimentResult` nesne `documentSentiment` `sentenceSentiments` , başarılı olursa, veya değilse `errorMessage` . 

```java
static void sentimentAnalysisExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had the best day of my life. I wish you were there with me.";

    DocumentSentiment documentSentiment = client.analyzeSentiment(text);
    System.out.printf(
        "Recognized document sentiment: %s, positive score: %s, neutral score: %s, negative score: %s.%n",
        documentSentiment.getSentiment(),
        documentSentiment.getConfidenceScores().getPositive(),
        documentSentiment.getConfidenceScores().getNeutral(),
        documentSentiment.getConfidenceScores().getNegative());

    for (SentenceSentiment sentenceSentiment : documentSentiment.getSentences()) {
        System.out.printf(
            "Recognized sentence sentiment: %s, positive score: %s, neutral score: %s, negative score: %s.%n",
            sentenceSentiment.getSentiment(),
            sentenceSentiment.getConfidenceScores().getPositive(),
            sentenceSentiment.getConfidenceScores().getNeutral(),
            sentenceSentiment.getConfidenceScores().getNegative());
        }
    }
```

### <a name="output"></a>Çıktı

```console
Recognized document sentiment: positive, positive score: 1.0, neutral score: 0.0, negative score: 0.0.
Recognized sentence sentiment: positive, positive score: 1.0, neutral score: 0.0, negative score: 0.0.
Recognized sentence sentiment: neutral, positive score: 0.21, neutral score: 0.77, negative score: 0.02.
```

## <a name="language-detection"></a>Dil algılama

`detectLanguageExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `detectLanguage()` işlevini çağırın. Döndürülen `DetectLanguageResult` nesne, algılanan birincil dili, başarılı olursa algılanan diğer dillerin bir listesini ya da olmadığını içerir `errorMessage` .

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. `countryHint`Parametresini 2 harfli bir ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz `countryHint = ""` . Farklı bir varsayılan ayarlamak için, özelliği ayarlayın `TextAnalyticsClientOptions.DefaultCountryHint` ve istemcinin başlatılması sırasında geçirin.

```java
static void detectLanguageExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Ce document est rédigé en Français.";

    DetectedLanguage detectedLanguage = client.detectLanguage(text);
    System.out.printf("Detected primary language: %s, ISO 6391 name: %s, score: %.2f.%n",
        detectedLanguage.getName(),
        detectedLanguage.getIso6391Name(),
        detectedLanguage.getConfidenceScore());
}
```

### <a name="output"></a>Çıktı

```console
Detected primary language: French, ISO 6391 name: fr, score: 1.00.
```
## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

> [!NOTE]
> Şu sürümde `3.0` :
> * NER kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama, NER 'den ayrı bir istek.

`recognizeEntitiesExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `recognizeEntities()` işlevini çağırın. Döndürülen `RecognizeEntitiesResult` nesne `NamedEntity` , başarılı olursa bir liste veya değilse bir listesi içerir `errorMessage` .

```java
static void recognizeEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had a wonderful trip to Seattle last week.";

    for (CategorizedEntity entity : client.recognizeEntities(text)) {
        System.out.printf(
            "Recognized entity: %s, entity category: %s, entity sub-category: %s, score: %s.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubcategory(),
            entity.getConfidenceScore());
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized entity: trip, entity category: Event, entity sub-category: null, score: 0.61.
Recognized entity: Seattle, entity category: Location, entity sub-category: GPE, score: 0.82.
Recognized entity: last week, entity category: DateTime, entity sub-category: DateRange, score: 0.8.
```

## <a name="entity-linking"></a>Varlık bağlama

`recognizeLinkedEntitiesExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `recognizeLinkedEntities()` işlevini çağırın. Döndürülen `RecognizeLinkedEntitiesResult` nesne `LinkedEntity` , başarılı olursa bir liste veya değilse bir listesi içerir `errorMessage` . Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları `LinkedEntity` nesne listesi olarak bir nesne altında gruplandırılır `LinkedEntityMatch` .

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
            linkedEntity.getDataSourceEntityId(),
            linkedEntity.getUrl(),
            linkedEntity.getDataSource());
        System.out.printf("Matches:%n");
        for (LinkedEntityMatch linkedEntityMatch : linkedEntity.getMatches()) {
            System.out.printf("Text: %s, Score: %.2f%n",
            linkedEntityMatch.getText(),
            linkedEntityMatch.getConfidenceScore());
        }
    }
}
```

### <a name="output"></a>Çıktı

```console
Linked Entities:
Name: Altair 8800, ID: Altair 8800, URL: https://en.wikipedia.org/wiki/Altair_8800, Data Source: Wikipedia.
Matches:
Text: Altair 8800, Score: 0.88
Name: Bill Gates, ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates, Data Source: Wikipedia.
Matches:
Text: Bill Gates, Score: 0.63
Text: Gates, Score: 0.63
Name: Paul Allen, ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen, Data Source: Wikipedia.
Matches:
Text: Paul Allen, Score: 0.60
Name: Microsoft, ID: Microsoft, URL: https://en.wikipedia.org/wiki/Microsoft, Data Source: Wikipedia.
Matches:
Text: Microsoft, Score: 0.55
Text: Microsoft, Score: 0.55
Name: April 4, ID: April 4, URL: https://en.wikipedia.org/wiki/April_4, Data Source: Wikipedia.
Matches:
Text: April 4, Score: 0.32
Name: BASIC, ID: BASIC, URL: https://en.wikipedia.org/wiki/BASIC, Data Source: Wikipedia.
Matches:
Text: BASIC, Score: 0.33
```
## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

`extractKeyPhrasesExample()`Daha önce oluşturduğunuz istemciyi alan adlı yeni bir işlev oluşturun ve `extractKeyPhrases()` işlevini çağırın. Döndürülen `ExtractKeyPhraseResult` nesne, başarılı olursa anahtar tümceciklerin bir listesini içerir ya da `errorMessage` böyle değildir.

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
