---
title: 'Hızlı başlangıç: node. js için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Node. js için v3 Metin Analizi istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.reviewer: sumeh, assafi
ms.openlocfilehash: 8bcc919aee7548e8596d1f44c8a357d3f84dfb14
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82095986"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

[v3 başvuru belgeleri](https://aka.ms/azsdk-js-textanalytics-ref-docs) | [v3 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [V3 paketi (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics) | [v3 örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples)


#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[v2 başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics) | [v2 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [v2 paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-textanalytics) | [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

---

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Node. js](https://nodejs.org/)' nin geçerli sürümü.
* Azure aboneliğiniz olduktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, anahtarınızı ve uç noktanızı almak için"  target="_blank">Azure Portal bir metin analizi kaynağı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> oluşturun metin analizi bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * Hizmeti denemek ve daha sonra üretime yönelik ücretli`F0`bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp 

cd myapp
```

Bir `package.json` dosya `npm init` ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın. 

```console
npm init
```
### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`@azure/ai-text-analytics` NPM paketlerini yüklerken:

```console
npm install --save @azure/ai-text-analytics@1.0.0-preview.4
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/TextAnalytics/text-analytics-v3-client-library.js)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

`@azure/cognitiveservices-textanalytics` NPM paketlerini yüklerken:

```console
npm install --save @azure/cognitiveservices-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js)bulabilirsiniz. 

---

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.
Adlı `index.js` bir dosya oluşturun ve aşağıdakileri ekleyin:

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

```javascript
"use strict";

const { TextAnalyticsClient, AzureKeyCredential } = require("@azure/ai-text-analytics");
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

```javascript
"use strict";

const { TextAnalyticsClient, CognitiveServicesCredential } = require("@azure/cognitiveservices-textanalytics");
```
---

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```javascript
const key = '<paste-your-text-analytics-key-here>';
const endpoint = '<paste-your-text-analytics-endpoint-here>';
```

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak `TextAnalyticsClient` Azure 'da kimlik doğrulayan bir nesnedir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar.

Metin, kullanılan yöntemine bağlı `documents`olarak `dictionary` `id`, `text`ve `language` özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. `text` Özniteliği, kaynak `language`olarak çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemci kimlik doğrulaması](#client-authentication)
* [Yaklaşım Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="client-authentication"></a>İstemci Kimlik Doğrulaması

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Anahtarınızla ve `TextAnalyticsClient` uç noktanızla parametreler olarak yeni bir nesne oluşturun.

```javascript
const textAnalyticsClient = new TextAnalyticsClient(endpoint,  new AzureKeyCredential(key));
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Parametresi olarak ve [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient) `credentials` `endpoint` ile yeni bir TextAnalyticsClient nesnesi oluşturun.

[!code-javascript[Authentication and client creation](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `analyzeSentiment()` metodunu çağırın ve döndürülen `SentimentBatchResult` nesneyi alın. Sonuçlar listesinde yineleme yapın ve her belgenin KIMLIĞINI, belge düzeyi yaklaşımını güven puanlarına göre yazdırın. Sonuç, her belge için kenar boşlukları, uzunluk ve güvenirlik puanlarıyla birlikte tümce düzeyinde yaklaşım içerir.

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

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Çözümlemek istediğiniz belgeleri içeren sözlük nesnelerinin bir listesini oluşturun. İstemcinin [Sentiment ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#sentiment-models-textanalyticsclientsentimentoptionalparams-) yöntemini çağırın ve döndürülen [Sentimentbatchresult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/sentimentbatchresult)'ı alın. Sonuçlar listesinde yineleme yapın ve her belgenin KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

[!code-javascript[Sentiment analysis](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=sentimentAnalysis)]

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
[ { id: '1', score: 0.87 } ]
[ { id: '2', score: 0.11 } ]
[ { id: '3', score: 0.44 } ]
[ { id: '4', score: 1.00 } ]
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `detectLanguage()` metodunu çağırın ve döndürülen geri `DetectLanguageResultCollection`alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ilgili birincil dille yazdırın.

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

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Primary Language French
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Belgelerinizi içeren sözlük nesnelerinin bir listesini oluşturun. İstemcinin [DetectLanguage ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#detectlanguage-models-textanalyticsclientdetectlanguageoptionalparams-) yöntemini çağırın ve döndürülen [languagebatchresult sonucunu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/languagebatchresult)alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve dilini yazdırın.

[!code-javascript[Language detection](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=languageDetection)]

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

> [!NOTE]
> Şu sürümde `3.0-preview`:
> * Varlık bağlama, NER 'den ayrı bir istek.

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeEntities()` metodunu çağırın ve `RecognizeEntitiesResult` nesneyi alın. Sonuçlar listesinde yineleme yapın ve varlık adı, türü, alt tür, kaydırma, uzunluk ve puanı yazdırın.

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

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

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

## <a name="entity-linking"></a>Varlık Bağlama

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeLinkedEntities()` metodunu çağırın ve `RecognizeLinkedEntitiesResult` nesneyi alın. Sonuçlar listesinde yineleme yapın ve varlık adı, KIMLIĞI, veri kaynağı, URL ve eşleşmeleri yazdırın. `matches` Dizideki her nesne, bu eşleşme için uzaklığa, uzunluğa ve puana sahip olur.

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

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

> [!NOTE]
> Sürüm 2,1 ' de, varlık bağlama, NER yanıtına dahil edilir.

Belgelerinizi içeren bir nesne listesi oluşturun. İstemcinin [Entities ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#entities-models-textanalyticscliententitiesoptionalparams-) yöntemini çağırın ve [Entitiesbatchresult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/entitiesbatchresult) nesnesini alın. Sonuçlar listesinde yineleme yapın ve her belge KIMLIĞINI yazdırın. Algılanan her varlık için, bu dosyanın visede adını, tür ve alt türlerini (varsa) ve özgün metindeki konumları yazdırın.

[!code-javascript[Entity recognition](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=entityRecognition)]

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

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

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `extractKeyPhrases()` metodunu çağırın ve döndürülen `ExtractKeyPhrasesResult` nesneyi alın. Sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan anahtar tümceleri yazdırın.

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

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Key Phrases: cat,veterinarian
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Belgelerinizi içeren bir nesne listesi oluşturun. İstemcinin [keyPhrases ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#keyphrases-models-textanalyticsclientkeyphrasesoptionalparams-) yöntemini çağırın ve döndürülen [KeyPhraseBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/keyphrasebatchresult) nesnesini alın. Sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan anahtar tümceleri yazdırın.

[!code-javascript[Key phrase extraction](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyPhraseExtraction)]

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

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

Uygulamayı hızlı başlangıç dosyanızdaki `node` komutla çalıştırın.

```console
node index.js
```
