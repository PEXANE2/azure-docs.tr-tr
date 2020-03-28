---
title: 'Quickstart: Node.js için Text Analytics v3 istemci kitaplığı | Microsoft Dokümanlar'
description: Node.js için v3 Text Analytics istemci kitaplığı ile başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.reviewer: sumeh, assafi
ms.openlocfilehash: a0616a8a2cc401b6b4c42c9882c14da2f123c6df
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79481883"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

[v3 Referans dokümantasyon](https://aka.ms/azsdk-js-textanalytics-ref-docs) | [v3 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [v3 Paketi (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics) | [v3 Örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples)


#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[v2 Referans dokümantasyon](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics) | [v2 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [v2 Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-textanalytics) | [v2 Örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Düğüm.js](https://nodejs.org/)geçerli sürümü .
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun. Dağıtıladıktan sonra **kaynağa git'i**tıklatın.
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Anahtarınızı ve bitiş noktanızı daha sonra hızlı başlatmada aşağıdaki koda yapıştıracaksınız.
    * Hizmeti denemek için ücretsiz`F0`fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp 

cd myapp
```

Bir `npm init` dosya ile bir düğüm uygulaması oluşturmak için komutu çalıştırın. `package.json` 

```console
npm init
```
### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

NPM `@azure/ai-text-analytics` paketlerini yükleyin:

```console
npm install --save @azure/ai-text-analytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/TextAnalytics/text-analytics-v3-client-library.js)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

NPM `@azure/cognitiveservices-textanalytics` paketlerini yükleyin:

```console
npm install --save @azure/cognitiveservices-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js)bulabilirsiniz. 

---

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.
Adlandırılmış `index.js` bir dosya oluşturun ve aşağıdakileri ekleyin:

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

```javascript
"use strict";

const { TextAnalyticsClient, TextAnalyticsApiKeyCredential } = require("@azure/ai-text-analytics");
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

```javascript
"use strict";

const { TextAnalyticsClient, CognitiveServicesCredential } = require("@azure/cognitiveservices-textanalytics");
```
---

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```javascript
const key = '<paste-your-text-analytics-key-here>';
const endpoint = `<paste-your-text-analytics-endpoint-here>`;
```

## <a name="object-model"></a>Nesne modeli

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir `TextAnalyticsClient` nesnedir. İstemci, metni tek bir dize veya toplu iş olarak çözümleme için çeşitli yöntemler sağlar.

Metin, kullanılan yönteme bağlı olarak `documents`, `dictionary` , ve `id` `text` `language` özniteliklerin birleşimini içeren nesneler listesi olarak API'ye gönderilir. Öznitelik, `text` metnin kaynağında `language`çözümlenecek metni depolar ve herhangi bir değer `id` olabilir. 

Yanıt nesnesi, her belge için çözümleme bilgilerini içeren bir listedir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemci Kimlik Doğrulaması](#client-authentication)
* [Duygusallık Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık tanıma](#named-entity-recognition-ner)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümcecik çıkarma](#key-phrase-extraction)

## <a name="client-authentication"></a>İstemci Kimlik Doğrulaması

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Parametre `TextAnalyticsClient` olarak anahtar ve bitiş noktası ile yeni bir nesne oluşturun.

```javascript
const textAnalyticsClient = new TextAnalyticsClient(endpoint,  new TextAnalyticsApiKeyCredential(key));
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Parametre li `credentials` ve `endpoint` parametre olarak yeni bir [TextAnalyticsClient nesnesi](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient) oluşturun.

[!code-javascript[Authentication and client creation](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `analyzeSentiment()` yöntemini arayın `SentimentBatchResult` ve döndürülen nesneyi alın. Sonuç listesini yineleyin ve her belgenin kimliğini, belge düzeyi duyarlılığını güven puanlarıyla yazdırın. Her belge için sonuç, uzaklıklar, uzunluk ve güven puanlarıyla birlikte cümle düzeyi duyarlılığını içerir.

```javascript
async function sentimentAnalysis(client){

    const sentimentInput = [
        "I had the best day of my life. I wish you were there with me."
    ];
    const sentimentResult = await client.analyzeSentiment(sentimentInput);

    sentimentResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Sentiment: ${document.sentiment}`);
        console.log(`\tDocument Scores:`);
        console.log(`\t\tPositive: ${document.confidenceScores.positive.toFixed(2)} \tNegative: ${document.confidenceScores.negative.toFixed(2)} \tNeutral: ${document.confidenceScores.neutral.toFixed(2)}`);
        console.log(`\tSentences Sentiment(${document.sentences.length}):`);
        document.sentences.forEach(sentence => {
            console.log(`\t\tSentence sentiment: ${sentence.sentiment}`)
            console.log(`\t\tSentences Scores:`);
            console.log(`\t\tPositive: ${sentence.confidenceScores.positive.toFixed(2)} \tNegative: ${sentence.confidenceScores.negative.toFixed(2)} \tNeutral: ${sentence.confidenceScores.neutral.toFixed(2)}`);
        });
    });
}
sentimentAnalysis(textAnalyticsClient)
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Sentiment: positive
        Document Scores:
                Positive: 1.00  Negative: 0.00  Neutral: 0.00
        Sentences Sentiment(2):
                Sentence sentiment: positive
                Sentences Scores:
                Positive: 1.00  Negative: 0.00  Neutral: 0.00
                Sentence sentiment: neutral
                Sentences Scores:
                Positive: 0.21  Negative: 0.02  Neutral: 0.77
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Çözümlemek istediğiniz belgeleri içeren sözlük nesnelerinin bir listesini oluşturun. Müşterinin [duyarlılık()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#sentiment-models-textanalyticsclientsentimentoptionalparams-) yöntemini arayın ve iade edilen [SentimentBatchResult'ı](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/sentimentbatchresult)alın. Sonuç listesini titretin ve her belgenin kimliğini ve duyarlılık puanını yazdırın. 0'a yakın bir puan olumsuz bir duyguyu gösterirken, 1'e yakın bir puan olumlu bir duyguyu gösterir.

[!code-javascript[Sentiment analysis](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=sentimentAnalysis)]

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
[ { id: '1', score: 0.87 } ]
[ { id: '2', score: 0.11 } ]
[ { id: '3', score: 0.44 } ]
[ { id: '4', score: 1.00 } ]
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `detectLanguage()` yöntemini arayın `DetectLanguageResultCollection`ve döndürülenleri alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ilgili birincil dille yazdırın.

```javascript
async function languageDetection(client) {

    const languageInputArray = [
        "Ce document est rédigé en Français."
    ];
    const languageResult = await client.detectLanguage(languageInputArray);

    languageResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tPrimary Language ${document.primaryLanguage.name}`)
    });
}
languageDetection(textAnalyticsClient);
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Primary Language French
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Belgelerinizi içeren sözlük nesnelerinin bir listesini oluşturun. İstemcinin [detectLanguage()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#detectlanguage-models-textanalyticsclientdetectlanguageoptionalparams-) yöntemini arayın ve döndürülen [LanguageBatchResult'ı](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/languagebatchresult)alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve dilini yazdırın.

[!code-javascript[Language detection](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=languageDetection)]

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

> [!NOTE]
> Sürümde `3.0-preview`:
> * NER, kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama NER'den ayrı bir istektir.

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `recognizeEntities()` yöntemini arayın `RecognizeEntitiesResult` ve nesneyi alın. Sonuç listesini yineleyin ve varlık adını, türünü, alt türünü, ofset,uzunluk ve skoru yazdırın.

```javascript
async function entityRecognition(client){

    const entityInputs = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800",
        "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."
    ];
    const entityResults = await client.recognizeEntities(entityInputs);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.text} \tCategory: ${entity.category} \tSubcategory: ${entity.subCategory ? entity.subCategory : "N/A"}`);
            console.log(`\tScore: ${entity.score}`);
        });
    });
}
entityRecognition(textAnalyticsClient);
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Score: 1
        Name: Bill Gates        Category: Person        Subcategory: N/A
        Score: 0.67
        Name: Paul Allen        Category: Person        Subcategory: N/A
        Score: 0.81
        Name: April 4, 1975     Category: DateTime      Subcategory: Date
        Score: 0.8
        Name: interpreters      Category: PersonType    Subcategory: N/A
        Score: 0.6
        Name: 8800      Category: Quantity      Subcategory: Number
        Score: 0.8
Document ID: 1
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Score: 0.96
        Name: Redmond   Category: Location      Subcategory: GPE
        Score: 0.09
        Name: 21        Category: Quantity      Subcategory: Number
        Score: 0.8
        Name: Seattle   Category: Location      Subcategory: GPE
        Score: 0.31
```

## <a name="using-ner-to-detect-personal-information"></a>Kişisel bilgileri algılamak için NER'i kullanma

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `recognizePiiEntities()` yöntemini arayın `EntitiesBatchResult` ve nesneyi alın. Sonuç listesini yineleyin ve varlık adını, türünü, alt türünü, ofset,uzunluk ve skoru yazdırın.


```javascript
async function entityPiiRecognition(client){

    const entityPiiInput = [
        "Insurance policy for SSN on file 123-12-1234 is here by approved."
    ];
    const entityPiiResults = await client.recognizePiiEntities(entityPiiInput);

    entityPiiResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.text} \tCategory: ${entity.category} \tSubcategory: ${entity.subCategory ? entity.subCategory : "N/A"}`);
            console.log(`\tScore: ${entity.score}`);
        });
    });
}
entityPiiRecognition(textAnalyticsClient);
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: 123-12-1234       Category: U.S. Social Security Number (SSN)     Subcategory: N/A
        Score: 0.85
```

## <a name="entity-linking"></a>Varlık Bağlama

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `recognizeLinkedEntities()` yöntemini arayın `RecognizeLinkedEntitiesResult` ve nesneyi alın. Sonuç listesini yineleyin ve varlık adını, kimliği, veri kaynağını, url'yi ve eşleşmeleri yazdırın. Dizideki `matches` her nesne, o maç için ofset, uzunluk ve skor içerir.

```javascript
async function linkedEntityRecognition(client){

    const linkedEntityInput = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800. During his career at Microsoft, Gates held the positions of chairman, chief executive officer, president and chief software architect, while also being the largest individual shareholder until May 2014."
    ];
    const entityResults = await client.recognizeLinkedEntities(linkedEntityInput);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.name} \tID: ${entity.dataSourceEntityId} \tURL: ${entity.url} \tData Source: ${entity.dataSource}`);
            console.log(`\tMatches:`)
            entity.matches.forEach(match => {
                console.log(`\t\tText: ${match.text} \tScore: ${match.score.toFixed(2)}`);
            });
        });
    });
}
linkedEntityRecognition(textAnalyticsClient);
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Altair 8800       ID: Altair 8800         URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800       Score: 0.78
        Name: Bill Gates        ID: Bill Gates  URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates        Score: 0.55
                Text: Gates     Score: 0.55
        Name: Paul Allen        ID: Paul Allen  URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen        Score: 0.53
        Name: Microsoft         ID: Microsoft   URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft         Score: 0.47
                Text: Microsoft         Score: 0.47
        Name: April 4   ID: April 4     URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4   Score: 0.25
        Name: BASIC     ID: BASIC       URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC     Score: 0.28
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

> [!NOTE]
> Sürüm 2.1'de, varlık bağlantısı NER yanıtına dahildir.

Belgelerinizi içeren nesnelerin bir listesini oluşturun. İstemcinin [varlıklarını arayın()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#entities-models-textanalyticscliententitiesoptionalparams-) yöntemini ve [VarlıklarBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/entitiesbatchresult) nesnesini alın. Sonuç listesini titretin ve her belgenin kimliğini yazdırın. Algılanan her varlık için wikipedia adını, türü ve alt türlerini (varsa) ve özgün metindeki konumları yazdırın.

[!code-javascript[Entity recognition](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=entityRecognition)]

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 1
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 0, Length: 9,   Score: 1.0
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
    Offset: 25, Length: 10, Score: 0.999847412109375
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
    Offset: 40, Length: 10, Score: 0.9988409876823425
    Name: April 4,  Type: Other,    Sub-Type: N/A
    Offset: 54, Length: 7,  Score: 0.8
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
    Offset: 54, Length: 13, Score: 0.8
    Name: BASIC,    Type: Other,    Sub-Type: N/A
    Offset: 89, Length: 5,  Score: 0.8
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
    Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 21, Length: 9,  Score: 0.999755859375
    Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
    Offset: 60, Length: 7,  Score: 0.9911284446716309
    Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
    Offset: 71, Length: 13, Score: 0.8
    Name: Seattle,  Type: Location, Sub-Type: N/A
    Offset: 88, Length: 7,  Score: 0.9998779296875
```

---

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dizi dize oluşturun. İstemcinin `extractKeyPhrases()` yöntemini arayın `ExtractKeyPhrasesResult` ve döndürülen nesneyi alın. Sonuçları titretin ve her belgenin kimliğini ve algılanan anahtar tümcecikleri yazdırın.

```javascript
async function keyPhraseExtraction(client){

    const keyPhrasesInput = [
        "My cat might need to see a veterinarian.",
    ];
    const keyPhraseResult = await client.extractKeyPhrases(keyPhrasesInput);
    
    keyPhraseResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Key Phrases: ${document.keyPhrases}`);
    });
}
keyPhraseExtraction(textAnalyticsClient);
```

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Key Phrases: cat,veterinarian
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Belgelerinizi içeren nesnelerin bir listesini oluşturun. İstemcinin [keyPhrases()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#keyphrases-models-textanalyticsclientkeyphrasesoptionalparams-) yöntemini arayın ve döndürülen [KeyPhraseBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/keyphrasebatchresult) nesnesini alın. Sonuçları titretin ve her belgenin kimliğini ve algılanan anahtar tümcecikleri yazdırın.

[!code-javascript[Key phrase extraction](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyPhraseExtraction)]

Kodunuzu konsol `node index.js` pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
[
    { id: '1', keyPhrases: [ '幸せ' ] }
    { id: '2', keyPhrases: [ 'Stuttgart', "hotel", "Fahrt", "Fu" ] }
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] }
    { id: '3', keyPhrases: [ 'fútbol' ] }
]
```

---

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `node` komutla uygulamayı çalıştırın.

```console
node index.js
```
