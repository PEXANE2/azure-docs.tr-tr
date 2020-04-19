---
title: 'Quickstart: Java için Text Analytics v3 istemci kitaplığı | Microsoft Dokümanlar'
description: Java için v3 Text Analytics istemci kitaplığı ile başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/17/2020
ms.author: aahi
ms.reviewer: tasharm, assafi, sumeh
ms.openlocfilehash: 31afb7bc00250887841adccc8c3cc4dc69462d55
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642894"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://aka.ms/azsdk-java-textanalytics-ref-docs) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics) | [Paket](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.4) | [Örnekleri](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Java Geliştirme Kiti](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK) sürüm 8 veya üzeri
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun.  Dağıtıladıktan sonra **kaynağa git'i**tıklatın.
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Anahtarınızı ve bitiş noktanızı daha sonra hızlı başlatmada aşağıdaki koda yapıştıracaksınız.
    * Hizmeti denemek için ücretsiz`F0`fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="add-the-client-library"></a>İstemci kitaplığını ekleme

Tercih ettiğiniz IDE veya geliştirme ortamında bir Maven projesi oluşturun. Ardından projenizin *pom.xml* dosyasına aşağıdaki bağımlılığı ekleyin. [Diğer yapı araçları için](https://mvnrepository.com/artifact/com.azure/azure-ai-textanalytics/1.0.0-beta.4) uygulama sözdizimini çevrimiçi olarak bulabilirsiniz.

```xml
<dependencies>
     <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.4</version>
    </dependency>
</dependencies>
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/TextAnalytics/TextAnalyticsSamples.java)bulabilirsiniz. 

Adlandırılmış `TextAnalyticsSamples.java`bir Java dosyası oluşturun. Dosyayı açın ve `import` aşağıdaki ifadeleri ekleyin:

```java
import com.azure.core.credential.AzureKeyCredential;
import com.azure.ai.textanalytics.models.*;
import com.azure.ai.textanalytics.TextAnalyticsClientBuilder;
import com.azure.ai.textanalytics.TextAnalyticsClient;
```

Java dosyasında, yeni bir sınıf ekleyin ve azure kaynağınızın anahtarını ve bitiş noktasını aşağıda gösterildiği gibi ekleyin.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```java
public class TextAnalyticsSamples {
    private static String KEY = "<replace-with-your-text-analytics-key-here>";
    private static String ENDPOINT = "<replace-with-your-text-analytics-endpoint-here>";
}
```

Sınıfa aşağıdaki ana yöntemi ekleyin. Burada çağrılan yöntemleri daha sonra tanımlarsınız.

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

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan ve metni tek dizeleri veya toplu iş olarak kabul eden işlevler sağlayan bir `TextAnalyticsClient` nesnedir. API'ye eşzamanlı veya eşzamanlı olarak metin gönderebilirsiniz. Yanıt nesnesi, gönderdiğiniz her belgenin çözümleme bilgilerini içerir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Duygusallık Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar ifade ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Metin Analizi kaynağınızın anahtarı `TextAnalyticsClient` ve bitiş noktasıyla nesneyi anında oluşturmak için bir yöntem oluşturun.

```java
static TextAnalyticsClient authenticateClient(String key, String endpoint) {
    return new TextAnalyticsClientBuilder()
        .apiKey(new AzureKeyCredential(key))
        .endpoint(endpoint)
        .buildClient();
}
```

Programınızın `main()` yönteminde, istemciyi anında anımsamak için kimlik doğrulama yöntemini arayın.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Daha önce oluşturduğunuz istemciyi alan ve `sentimentAnalysisExample()` `analyzeSentiment()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `AnalyzeSentimentResult` nesne `documentSentiment` içerir `sentenceSentiments` ve başarılı `errorMessage` olursa, ya da değilse. 

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

Daha önce oluşturduğunuz istemciyi alan ve `detectLanguageExample()` `detectLanguage()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `DetectLanguageResult` nesne algılanan bir birincil dil, başarılı olursa algılanan diğer `errorMessage` dillerin bir listesini veya değilse bir içerir.

> [!Tip]
> Bazı durumlarda girişe göre dilleri ayrıştırmak zor olabilir. Parametreyi `countryHint` 2 harfli ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak API varsayılan countryHint olarak "ABD" kullanıyor, bu davranışı kaldırmak için bu değeri boş `countryHint = ""`dize için ayarlayarak bu parametreyi sıfırlayabilirsiniz. Farklı bir varsayılan ayarlamak `TextAnalyticsClientOptions.DefaultCountryHint` için özelliği ayarlayın ve istemcinin başlatma sırasında geçirin.

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
## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık tanıma (NER)

> [!NOTE]
> Sürümde `3.0-preview`:
> * NER, kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama NER'den ayrı bir istektir.

Daha önce oluşturduğunuz istemciyi alan ve `recognizeEntitiesExample()` `recognizeEntities()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `RecognizeEntitiesResult` nesne, başarılı `NamedEntity` olup olmadığının `errorMessage` bir listesini veya başarısız olup olmadığını içerir.

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

## <a name="entity-linking"></a>Varlık bağlama

Daha önce oluşturduğunuz istemciyi alan ve `recognizeLinkedEntitiesExample()` `recognizeLinkedEntities()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `RecognizeLinkedEntitiesResult` nesne, başarılı `LinkedEntity` olup olmadığının `errorMessage` bir listesini veya başarısız olup olmadığını içerir. Bağlı varlıklar benzersiz olarak tanımlandığından, aynı varlığın oluşumları `LinkedEntity` `LinkedEntityMatch` nesnelerin listesi olarak bir nesne altında gruplandırılır.

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

Daha önce oluşturduğunuz istemciyi alan ve `extractKeyPhrasesExample()` `extractKeyPhrases()` işlevini çağıran yeni bir işlev oluşturun. Döndürülen `ExtractKeyPhraseResult` nesne, başarılı olursa anahtar tümceciklerin `errorMessage` bir listesini veya başarısız olursa içerir.

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
