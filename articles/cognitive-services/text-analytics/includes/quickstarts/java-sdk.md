---
title: 'Hızlı Başlangıç: Java için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Java için Metin Analizi v3 istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.reviewer: tasharm, assafi, sumeh
ms.openlocfilehash: 11092b74c0256d298dece0f909d8d7dd241e7b13
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371350"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://aka.ms/azsdk-java-textanalytics-ref-docs) | [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics) | [Paket](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.3) | [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz olarak oluşturun](https://azure.microsoft.com/free/)
* [Java Geliştirme Seti](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK) sürüm 8 veya üstü
* Azure aboneliğinizi oluşturduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portalda <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Metin Analizi kaynağı oluşturun"  target="_blank">Metin Analizi kaynağı oluşturun <span class="docon docon-navigate-external x-hidden-focus"></span></a>. 
    * Uygulamanızı Metin Analizi API’sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu hızlı başlangıçta daha sonra yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="add-the-client-library"></a>İstemci kitaplığını ekleme

Tercih ettiğiniz IDE veya geliştirme ortamında bir Maven projesi oluşturun. Ardından aşağıdaki bağımlılığı projenizin *pom.xml* dosyasına ekleyin. [Diğer derleme araçlarına](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.3) yönelik uygulama söz dizimini çevrimiçi olarak bulabilirsiniz.

```xml
<dependencies>
     <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.3</version>
    </dependency>
</dependencies>
```

> [!TIP]
> Tüm hızlı başlangıç kodunu aynı anda görüntülemek mi istiyorsunuz? Bunu, bu hızlı başlangıçtaki kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/TextAnalytics/TextAnalyticsSamples.java)’da bulabilirsiniz. 

`TextAnalyticsSamples.java` adlı bir Java dosyası oluşturun. Dosyayı açıp aşağıdaki `import` deyimlerini ekleyin:

```java
import com.azure.ai.textanalytics.models.*;
import com.azure.ai.textanalytics.TextAnalyticsClientBuilder;
import com.azure.ai.textanalytics.TextAnalyticsClient;
```

Java dosyasına yeni bir sınıf ekleyin Azure kaynağınızın anahtarını ve uç noktasını aşağıda gösterildiği gibi ekleyin.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```java
public class TextAnalyticsSamples {
    private static String KEY = "<replace-with-your-text-analytics-key-here>";
    private static String ENDPOINT = "<replace-with-your-text-analytics-endpoint-here>";
}
```

Aşağıdaki ana yöntemi sınıfa ekleyin. Burada çağrılan yöntemleri daha sonra tanımlayacaksınız.

```java
public static void main(String[] args) {
    //You will create these methods later in the quickstart.
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

Metin Analizi istemcisi, anahtarınızı kullanarak Azure’da kimlik doğrulaması yapan ve metnin tek dizeler halinde veya bir toplu iş olarak kabul edilmesine olanak tanıyan işlevleri sunan bir `TextAnalyticsClient` nesnesidir. Metinleri API’ye zaman uyumlu veya zaman uyumsuz olarak gönderebilirsiniz. Yanıt nesnesi, gönderdiğiniz her belgeye yönelik analiz bilgilerini içerir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar ifade ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Anahtar ve Metin Analizi kaynağınıza yönelik uç noktasıyla `TextAnalyticsClient` nesnesinin örneğini oluşturmak için bir yöntem oluşturun.

```java
static TextAnalyticsClient authenticateClient(String key, String endpoint) {
    return new TextAnalyticsClientBuilder()
        .apiKey(new TextAnalyticsApiKeyCredential(key))
        .endpoint(endpoint)
        .buildClient();
}
```

İstemcinin örneğini oluşturmak için programınızın `main()` yönteminde kimlik doğrulaması yöntemini çağırın.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Önceden oluşturduğunuz istemciyi alan ve bunun `analyzeSentiment()` işlevini çağıran `sentimentAnalysisExample()` adlı yeni bir işlev oluşturun. Çağrı başarılı olursa döndürülen `AnalyzeSentimentResult` nesnesi `documentSentiment` ve `sentenceSentiments` içerir. Çağrı başarısız olursa nesne `errorMessage` içerir. 

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

Önceden oluşturduğunuz istemciyi alan ve bunun `detectLanguage()` işlevini çağıran `detectLanguageExample()` adlı yeni bir işlev oluşturun. Döndürülen `DetectLanguageResult` nesnesi başarılı olursa algılanan birincil dili ve algılanan diğer dillerin listesini içerir. Başarısız olursa nesne `errorMessage` içerir.

> [!Tip]
> Bazı durumlarda girişi temel alarak dilleri netleştirmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `countryHint` parametresini kullanabilirsiniz. API, countryHint için varsayılan olarak “US”değerini kullanır. Bu davranışı kaldırmak için bu değeri boş `countryHint = ""` dizesi olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak için `TextAnalyticsClientOptions.DefaultCountryHint` özelliğini ayarlayın ve müşterinin başlatma işlemi esnasında bunu geçirin.

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
## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

> [!NOTE]
> `3.0-preview` sürümünde:
> * NER, kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama, NER’den ayrı bir istektir.

Önceden oluşturduğunuz istemciyi alan ve bunun `recognizeEntities()` işlevini çağıran `recognizeEntitiesExample()` adlı yeni bir işlev oluşturun. Çağrı başarılı olursa döndürülen `RecognizeEntitiesResult` nesnesi `NamedEntity` listesini içerir. Çağrı başarısız olursa nesne `errorMessage` içerir.

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
            entity.getSubCategory(),
            entity.getConfidenceScore());
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized entity: Seattle, entity category: Location, entity sub-category: GPE, score: 0.92.
Recognized entity: last week, entity category: DateTime, entity sub-category: DateRange, score: 0.8.
```

## <a name="using-ner-to-recognize-personal-information"></a>Kişisel bilgileri tanımak için NER kullanma

Önceden oluşturduğunuz istemciyi alan ve bunun `recognizePiiEntities()` işlevini çağıran `recognizePIIEntitiesExample()` adlı yeni bir işlev oluşturun. Çağrı başarılı olursa döndürülen `RecognizePiiEntitiesResult` nesnesi `NamedEntity` listesini içerir. Çağrı başarısız olursa nesne `errorMessage` içerir. 

```java
static void recognizePIIEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Insurance policy for SSN on file 123-12-1234 is here by approved.";

    for (PiiEntity entity : client.recognizePiiEntities(text)) {
        System.out.printf(
            "Recognized personal identifiable information entity: %s, entity category: %s, %nentity sub-category: %s, score: %s.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubCategory(),
            entity.getConfidenceScore());
    }
}
```

### <a name="output"></a>Çıktı

```console
Recognized personal identifiable information entity: 123-12-1234, entity category: U.S. Social Security Number (SSN), 
entity sub-category: null, score: 0.85.
```

## <a name="entity-linking"></a>Varlık bağlama

Önceden oluşturduğunuz istemciyi alan ve bunun `recognizeLinkedEntities()` işlevini çağıran `recognizeLinkedEntitiesExample()` adlı yeni bir işlev oluşturun. Çağrı başarılı olursa döndürülen `RecognizeLinkedEntitiesResult` nesnesi `LinkedEntity` listesini içerir. Çağrı başarısız olursa nesne `errorMessage` içerir. Bağlı varlıklar benzersiz olarak tanımlandıkları için aynı varlıkların oluşumu `LinkedEntityMatch` nesnelerinin listesi olarak bir `LinkedEntity` nesnesinde gruplandırılır.

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
        for (LinkedEntityMatch linkedEntityMatch : linkedEntity.getLinkedEntityMatches()) {
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
Text: Altair 8800, Score: 0.78
Name: Bill Gates, ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates, Data Source: Wikipedia.
Matches:
Text: Bill Gates, Score: 0.55
Text: Gates, Score: 0.55
Name: Paul Allen, ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen, Data Source: Wikipedia.
Matches:
Text: Paul Allen, Score: 0.53
Name: Microsoft, ID: Microsoft, URL: https://en.wikipedia.org/wiki/Microsoft, Data Source: Wikipedia.
Matches:
Text: Microsoft, Score: 0.47
Text: Microsoft, Score: 0.47
Name: April 4, ID: April 4, URL: https://en.wikipedia.org/wiki/April_4, Data Source: Wikipedia.
Matches:
Text: April 4, Score: 0.25
Name: BASIC, ID: BASIC, URL: https://en.wikipedia.org/wiki/BASIC, Data Source: Wikipedia.
Matches:
Text: BASIC, Score: 0.28
```
## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

Önceden oluşturduğunuz istemciyi alan ve bunun `extractKeyPhrases()` işlevini çağıran `extractKeyPhrasesExample()` adlı yeni bir işlev oluşturun. Çağrı başarılı olursa döndürülen `ExtractKeyPhraseResult` nesnesi anahtar ifadelerin listesini içerir. Çağrı başarısız olursa nesne `errorMessage` içerir.

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
