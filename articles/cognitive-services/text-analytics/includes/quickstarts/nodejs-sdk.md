---
title: 'Hızlı Başlangıç: Node.js için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Node.js için v3 Metin Analizi istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.reviewer: sumeh, assafi
ms.openlocfilehash: 4ce2da3070105f6e098373108164b6f590d423c6
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78925455"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

[v3 Başvuru belgeleri](https://aka.ms/azsdk-js-textanalytics-ref-docs) | [v3 Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [v3 Paketi (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics) | [v3 Örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples)


#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[v2 Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics) | [v2 Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [v2 Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-textanalytics) | [v2 Örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz olarak oluşturun](https://azure.microsoft.com/free/)
* [Node.js](https://nodejs.org/)’nin geçerli sürümü.
* Azure aboneliğinizi oluşturduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portalda <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Metin Analizi kaynağı oluşturun"  target="_blank">Metin Analizi kaynağı oluşturun <span class="docon docon-navigate-external x-hidden-focus"></span></a>. 
    * Uygulamanızı Metin Analizi API’sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu hızlı başlangıçta daha sonra yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

> [!NOTE]
> Metin Analizi istemci kitaplığının bu sürümünü [tarayıcınızda](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md) da çalıştırabilirsiniz.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin. 

```console
mkdir myapp && cd myapp
```

Bir `package.json` dosyası ile bir düğüm uygulaması oluşturmak için `npm init` komutunu çalıştırın. 

```console
npm init
```
### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

`@azure/ai-text-analytics` NPM paketlerini yükleme:

```console
npm install --save @azure/ai-text-analytics
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

`@azure/cognitiveservices-textanalytics` NPM paketlerini yükleme:

```console
npm install --save @azure/cognitiveservices-textanalytics
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilecek.

`index.js` adlı bir dosya oluşturun ve aşağıdaki kitaplıkları ekleyin:

---

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilecek.
`index.js` adlı bir dosya oluşturun ve aşağıdaki kitaplıkları ekleyin:

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

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

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```javascript
const key = '<paste-your-text-analytics-key-here>';
const endpoint = `<paste-your-text-analytics-endpoint-here>`;
```

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak Azure’da kimlik doğrulaması yapan bir `TextAnalyticsClient` nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metni çözümlemek için çeşitli yöntemler sağlar.

Metin, kullanılan yönteme bağlı olarak `id`, `text`ve `language` özniteliklerinin bir birleşimini içeren `dictionary` nesneleri olan bir `documents` listesi olarak API’ye gönderilir. `text` özniteliği, kaynak `language` içinde analiz edilecek metni depolar ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listedir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemci Kimlik Doğrulaması](#client-authentication)
* [Yaklaşım Analizi](#sentiment-analysis) 
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık tanıma](#named-entity-recognition-ner)
* [Varlık bağlama](#entity-linking)
* [Anahtar ifade ayıklama](#key-phrase-extraction)

## <a name="client-authentication"></a>İstemci Kimlik Doğrulaması

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Anahtarınızı ve uç noktanızı parametre olarak kullanarak yeni bir `TextAnalyticsClient` nesnesi oluşturun.

```javascript
const textAnalyticsClient = new TextAnalyticsClient(endpoint,  new TextAnalyticsApiKeyCredential(key));
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

`credentials` ve `endpoint` öğelerini bir parametre olarak kullanarak yeni bir [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient) nesnesi oluşturun.

[!code-javascript[Authentication and client creation](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=authentication)]

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `analyzeSentiment()` yöntemini çağırın ve döndürülen `SentimentBatchResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve her belgenin kimliğini ve belge düzeyi yaklaşımını güven puanları ile yazdırın. Sonuç, her belge için kenar boşlukları, uzunluk ve güvenirlik puanlarıyla birlikte tümce düzeyinde yaklaşımı içerir.

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
        console.log(`\t\tPositive: ${document.sentimentScores.positive.toFixed(2)} \tNegative: ${document.sentimentScores.negative.toFixed(2)} \tNeutral: ${document.sentimentScores.neutral.toFixed(2)}`);
        console.log(`\tSentences Sentiment(${document.sentences.length}):`);
        document.sentences.forEach(sentence => {
            console.log(`\t\tSentence sentiment: ${sentence.sentiment}`)
            console.log(`\t\tSentences Scores:`);
            console.log(`\t\tPositive: ${sentence.sentimentScores.positive.toFixed(2)} \tNegative: ${sentence.sentimentScores.negative.toFixed(2)} \tNeutral: ${sentence.sentimentScores.neutral.toFixed(2)}`);
            console.log(`\t\tLength: ${sentence.length}, Offset: ${sentence.offset}`);
        })
    });
}
sentimentAnalysis(textAnalyticsClient)
```

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

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
                Length: 30, Offset: 0
                Sentence sentiment: neutral
                Sentences Scores:
                Positive: 0.21  Negative: 0.02  Neutral: 0.77
                Length: 30, Offset: 31
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Çözümlemek istediğiniz belgeleri içeren sözlük nesnelerinin bir listesini oluşturun. İstemcinin [sentiment()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#sentiment-models-textanalyticsclientsentimentoptionalparams-) metodunu çağırın ve döndürülen [SentimentBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/sentimentbatchresult) öğesini alın. Sonuçlar listesinde yineleme yapın ve her belgenin kimliğini ve yaklaşım puanını yazdırın. 0’a yakın puanlar negatif yaklaşımı, 1’e yakın puanlar ise pozitif yaklaşımı gösterir.

[!code-javascript[Sentiment analysis](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=sentimentAnalysis)]

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
[ { id: '1', score: 0.87 } ]
[ { id: '2', score: 0.11 } ]
[ { id: '3', score: 0.44 } ]
[ { id: '4', score: 1.00 } ]
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `detectLanguage()` yöntemini çağırın ve döndürülen `DetectLanguageResultCollection` öğesini alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin kimliğini ilgili birincil dil ile yazdırın.

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

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Primary Language French
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Belgelerinizi içeren sözlük nesnelerinin bir listesini oluşturun. İstemcinin [detectLanguage()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#detectlanguage-models-textanalyticsclientdetectlanguageoptionalparams-) metodunu çağırın ve döndürülen [LanguageBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/languagebatchresult) öğesini alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin kimliğini ve dilini yazdırın.

[!code-javascript[Language detection](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=languageDetection)]

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

> [!NOTE]
> `3.0-preview` sürümünde:
> * NER, kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama, NER’den ayrı bir istektir.

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeEntities()` yöntemini çağırın ve `RecognizeEntitiesResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, tür, alt tür, uzaklık, uzunluk ve puanı yazdırın.

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
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Offset: 0, Length: 9    Score: 1
        Name: Bill Gates        Category: Person        Subcategory: N/A
        Offset: 25, Length: 10  Score: 0.67
        Name: Paul Allen        Category: Person        Subcategory: N/A
        Offset: 40, Length: 10  Score: 0.81
        Name: April 4, 1975     Category: DateTime      Subcategory: Date
        Offset: 54, Length: 13  Score: 0.8
        Name: interpreters      Category: PersonType    Subcategory: N/A
        Offset: 95, Length: 12  Score: 0.6
        Name: 8800      Category: Quantity      Subcategory: Number
        Offset: 123, Length: 4  Score: 0.8
Document ID: 1
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Offset: 21, Length: 9   Score: 0.96
        Name: Redmond   Category: Location      Subcategory: GPE
        Offset: 60, Length: 7   Score: 0.09
        Name: 21        Category: Quantity      Subcategory: Number
        Offset: 71, Length: 2   Score: 0.8
        Name: Seattle   Category: Location      Subcategory: GPE
```

## <a name="using-ner-to-detect-personal-information"></a>Kişisel bilgileri algılamak için NER kullanma

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizePiiEntities()` yöntemini çağırın ve `EntitiesBatchResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, tür, alt tür, uzaklık, uzunluk ve puanı yazdırın.


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
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityPiiRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: 123-12-1234       Category: U.S. Social Security Number (SSN)     Subcategory: N/A
        Offset: 33, Length: 11  Score: 0.85
```

## <a name="entity-linking"></a>Varlık Bağlama

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeLinkedEntities()` yöntemini çağırın ve `RecognizeLinkedEntitiesResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, kimliği, veri kaynağı, URL ve eşleşmeleri yazdırın. `matches` dizisindeki her nesne, bu eşleşme için uzaklığı, uzunluğu ve puanı içerir.

```javascript
async function linkedEntityRecognition(client){

    const linkedEntityInput = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800. During his career at Microsoft, Gates held the positions of chairman, chief executive officer, president and chief software architect, while also being the largest individual shareholder until May 2014."
    ];
    const entityResults = await client.recognizeLinkedEntities(linkedEntityInput);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.name} \tID: ${entity.id} \tURL: ${entity.url} \tData Source: ${entity.dataSource}`);
            console.log(`\tMatches:`)
            entity.matches.forEach(match => {
                console.log(`\t\tText: ${match.text}`);
                console.log(`\t\tOffset: ${match.offset}, Length: ${match.length} \tScore: ${match.score.toFixed(3)}`);
            });
        });
    });
}
linkedEntityRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Altair 8800       ID: Altair 8800         URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Offset: 116, Length: 11         Score: 0.777
        Name: Bill Gates        ID: Bill Gates  URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25, Length: 10  Score: 0.555
                Text: Gates
                Offset: 161, Length: 5  Score: 0.555
        Name: Paul Allen        ID: Paul Allen  URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40, Length: 10  Score: 0.533
        Name: Microsoft         ID: Microsoft   URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0, Length: 9    Score: 0.469
                Text: Microsoft
                Offset: 150, Length: 9  Score: 0.469
        Name: April 4   ID: April 4     URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54, Length: 7   Score: 0.248
        Name: BASIC     ID: BASIC       URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89, Length: 5   Score: 0.281
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

> [!NOTE]
> Sürüm 2.1’de, varlık bağlama NER yanıtına dahil edilir.

Belgelerinizi içeren nesnelerin bir listesini oluşturun. İstemcinin [entities()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#entities-models-textanalyticscliententitiesoptionalparams-) metodunu çağırın ve [EntitiesBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/entitiesbatchresult) nesnesini alın. Sonuçlar listesinde yineleme yapın ve her belgenin kimliğini yazdırın. Algılanan her varlık için, bu dosyanın wikipedia adını, tür ve alt türlerini (varsa) ve özgün metindeki konumlarını yazdırın.

[!code-javascript[Entity recognition](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=entityRecognition)]

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

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

#### <a name="version-30-preview"></a>[Sürüm 3.0-preview](#tab/version-3)

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `extractKeyPhrases()` yöntemini çağırın ve döndürülen `ExtractKeyPhrasesResult` nesnesini alın. Sonuçlar arasında yineleme yapın ve her belgenin kimliğini ve algılanan anahtar ifadeleri yazdırın.

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

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Key Phrases: cat,veterinarian
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Belgelerinizi içeren nesnelerin bir listesini oluşturun. İstemcinin [keyPhrases()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#keyphrases-models-textanalyticsclientkeyphrasesoptionalparams-) metodunu çağırın ve döndürülen [KeyPhraseBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/keyphrasebatchresult) nesnesini alın. Sonuçlar arasında yineleme yapın ve her belgenin kimliğini ve algılanan anahtar ifadeleri yazdırın.

[!code-javascript[Key phrase extraction](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyPhraseExtraction)]

Kodunuzu konsol pencerenizde `node index.js` ile çalıştırın.

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

Uygulamayı hızlı başlangıç dosyanızdaki `node` komutuyla çalıştırın.

```console
node index.js
```
