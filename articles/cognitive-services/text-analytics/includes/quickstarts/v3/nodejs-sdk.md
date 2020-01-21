---
title: 'Hızlı başlangıç: node. js için Metin Analizi v3 istemci kitaplığı | Microsoft Docs'
description: Node. js için v3 Metin Analizi istemci kitaplığı ile çalışmaya başlayın.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/13/2020
ms.author: aahi
ms.reviewer: sumeh, assafi
ms.openlocfilehash: 69899f521e73cb5af1af145a0915dbe1a017f307
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281213"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://aka.ms/azsdk-js-textanalytics-ref-docs) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [paketi (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics) | [örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples)

> [!NOTE]
> * Bu hızlı başlangıçta, iyileştirilmiş [yaklaşım Analizi](../../../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../../../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel bir önizleme içeren Metin Analizi istemci kitaplığının sürüm `3.0-preview` kullanılmaktadır.
> * Bu makaledeki kod, basitlik nedenlerle güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında performans ve ölçeklenebilirlik için diziler toplu olarak gönderilmesini öneririz. Örneğin, `Sentiment()`yerine `SentimentBatchAsync()` çağrılıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Node. js](https://nodejs.org/)' nin geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

Bir `package.json` dosyası ile bir düğüm uygulaması oluşturmak için `npm init` komutunu çalıştırın. 

```console
npm init
```
### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`@azure/cognitiveservices-textanalytics` NPM paketlerini yükler:

```console
npm install --save @azure/cognitiveservices-textanalytics
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilecektir.

`index.js` adlı bir dosya oluşturun ve aşağıdaki kitaplıkları ekleyin:

```javascript
"use strict";

const { TextAnalyticsClient, CognitiveServicesCredential } = require("@azure/cognitiveservices-textanalytics");
```

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info.md)]

```javascript
const key = '<paste-your-text-analytics-key-here>';
const endpoint = `<paste-your-text-analytics-endpoint-here>`;
```

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir `TextAnalyticsClient` nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar.

Metin, kullanılan yönteme bağlı olarak `id`, `text`ve `language` özniteliklerinin bir birleşimini içeren `dictionary` nesneler olan `documents`bir liste olarak API 'ye gönderilir. `text` özniteliği, kaynak `language`analiz edilecek metni depolar ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemci kimlik doğrulaması](#client-authentication)
* [Yaklaşım Analizi](#sentiment-analysis) (Genel Önizleme)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-public-preview) (Genel Önizleme)
* [Adlandırılmış varlık tanıma-kişisel bilgiler](#named-entity-recognition---personal-information-public-preview) (Genel Önizleme)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="client-authentication"></a>İstemci Kimlik Doğrulaması

Anahtarınızla ve uç noktanızla parametreler olarak yeni bir `TextAnalyticsClient` nesnesi oluşturun.

```javascript
const client = new TextAnalyticsClient(endpoint,  new CognitiveServicesCredential(key));
```

## <a name="sentiment-analysis-public-preview"></a>Yaklaşım Analizi (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan yaklaşım Analizi v3 içindir.

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `analyzeSentiment()` yöntemini çağırın ve döndürülen `SentimentBatchResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve her belgenin KIMLIĞINI, belge düzeyi yaklaşımını güven puanlarına göre yazdırın. Sonuç, her belge için kenar boşlukları, uzunluk ve güvenirlik puanlarıyla birlikte tümce düzeyinde yaklaşım içerir.

```javascript
async function sentimentAnalysis(client){

    const sentimentInput = [
        "I had the best day of my life. I wish you were there with me."
    ]

    const sentimentResult = await client.analyzeSentiment(sentimentInput);
    result.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Sentiment: ${document.sentiment}`);
        console.log(`\tDocument Scores:`);
        console.log(`\t\tPositive: ${document.documentScores.positive.toFixed(2)} \tNegative: ${document.documentScores.negative.toFixed(2)} \tNeutral: ${document.documentScores.neutral.toFixed(2)}`);
        console.log(`\tSentences Sentiment(${document.sentences.length}):`);
        document.sentences.forEach(sentence => {
            console.log(`\t\tSentence sentiment: ${sentence.sentiment}`)
            console.log(`\t\tSentences Scores:`);
            console.log(`\t\tPositive: ${sentence.sentenceScores.positive.toFixed(2)} \tNegative: ${sentence.sentenceScores.negative.toFixed(2)} \tNeutral: ${sentence.sentenceScores.neutral.toFixed(2)}`);
            console.log(`\t\tLength: ${sentence.length}, Offset: ${sentence.offset}`);
        })
    });
}
sentimentAnalysis(textAnalyticsClient)
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Sentiment: positive
        Document Scores:
                Positive: 0.61  Negative: 0.01  Neutral: 0.39
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

## <a name="language-detection"></a>Dil algılama

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `detectLanguages()` yöntemini çağırın ve döndürülen `DetectLanguageResult`alın. Ardından sonuçları yineleyin ve ilgili birincil ve algılanan dille birlikte her bir belgenin KIMLIĞINI yazdırın.

```javascript
async function languageDetection(client) {

    const languageInputArray = [
        "Ce document est rédigé en Français."
    ]

    const languageResult = await client.detectLanguages(languageInputArray);

    result.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tDetected Language ${language.name}`)
        );
        console.log(`\tPrimary Language ${document.primaryLanguage.name}`)
    });
}
languageDetection(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Detected Language French
        Primary Language French
```

## <a name="named-entity-recognition"></a>Adlandırılmış Varlık Tanıma

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 içindir.

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeEntities()` yöntemini çağırın ve `RecognizeEntitiesResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, türü, alt tür, kaydırma, uzunluk ve puanı yazdırın.

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
            console.log(`\tName: ${entity.text} \tType: ${entity.type} \tSub Type: ${entity.subtype != "" ? entity.subtype : "N/A"}`);
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Microsoft         Type: Organization      Sub Type: N/A
        Offset: 0, Length: 9    Score: 1
        Name: Bill Gates        Type: Person    Sub Type: N/A
        Offset: 25, Length: 10  Score: 0.999786376953125
        Name: Paul Allen        Type: Person    Sub Type: N/A
        Offset: 40, Length: 10  Score: 0.9988105297088623
        Name: April 4, 1975     Type: DateTime  Sub Type: Date
        Offset: 54, Length: 13  Score: 0.8
        Name: Altair    Type: Organization      Sub Type: N/A
        Offset: 116, Length: 6  Score: 0.7996330857276917
        Name: 8800      Type: Quantity  Sub Type: Number
        Offset: 123, Length: 4  Score: 0.8
Document ID: 1
        Name: Microsoft         Type: Organization      Sub Type: N/A
        Offset: 21, Length: 9   Score: 0.9837456345558167
        Name: 21        Type: Quantity  Sub Type: Number
        Offset: 71, Length: 2   Score: 0.8
```

## <a name="named-entity-recognition---personal-information-public-preview"></a>Adlandırılmış varlık tanıma-kişisel bilgiler (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 kullanan kişisel bilgileri tespit etmek içindir.

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizePiiEntities()` yöntemini çağırın ve `EntitiesBatchResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, türü, alt tür, kaydırma, uzunluk ve puanı yazdırın.


```javascript
async function entityPiiRecognition(client){

    const entityPiiInput = [
        "Insurance policy for SSN on file 123-12-1234 is here by approved."
    ]
    const entityResults = await client.recognizePiiEntities(entityPiiInput);

    result.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.text} \tType: ${entity.type} \tSub Type: ${entity.subtype != "" ? entity.subtype : "N/A"}`);
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityPiiRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: 123-12-1234       Type: U.S. Social Security Number (SSN)         Sub Type: N/A
        Offset: 33, Length: 11  Score: 0.85
```

## <a name="entity-linking"></a>Varlık Bağlama

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `recognizeLinkedEntities()` yöntemini çağırın ve `RecognizeLinkedEntitiesResult` nesnesini alın. Sonuçlar listesinde yineleme yapın ve varlık adı, KIMLIĞI, veri kaynağı, URL ve eşleşmeleri yazdırın. `matches` dizideki her nesne, bu eşleşme için uzaklığa, uzunluğa ve puana sahip olur.

```javascript
async function linkedEntityRecognition(client){

    const linkedEntityInput = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800. During his career at Microsoft, Gates held the positions of chairman, chief executive officer, president and chief software architect, while also being the largest individual shareholder until May 2014."
    ]
    const entityResults = await client.recognizeLinkedEntities(linkedEntityInput);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.name} \tID: ${entity.id} \tURL: ${entity.url} \tData Source: ${entity.dataSource}`);
            console.log(`\tMatches:`)
            entity.matches.forEach(match => {
                console.log(`\t\tText: ${match.text}`);
                console.log(`\t\tOffset: ${match.offset}, Length: ${match.length} \tScore: ${match.score.toFixed(3)}`);
            })
        });
    });
}
linkedEntityRecognition(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0
        Name: Altair 8800       ID: Altair 8800         URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Offset: 116, Length: 11         Score: 0.650
        Name: Bill Gates        ID: Bill Gates  URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25, Length: 10  Score: 0.243
                Text: Gates
                Offset: 161, Length: 5  Score: 0.243
        Name: Paul Allen        ID: Paul Allen  URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40, Length: 10  Score: 0.174
        Name: Microsoft         ID: Microsoft   URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0, Length: 9    Score: 0.196
                Text: Microsoft
                Offset: 150, Length: 9  Score: 0.196
        Name: April 4   ID: April 4     URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54, Length: 7   Score: 0.137
        Name: BASIC     ID: BASIC       URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89, Length: 5   Score: 0.052
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Çözümlemek istediğiniz belgeyi içeren bir dize dizisi oluşturun. İstemcinin `extractKeyPhrases()` yöntemini çağırın ve döndürülen `ExtractKeyPhrasesResult` nesnesini alın. Sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan anahtar tümceleri yazdırın.

```javascript
async function keyPhraseExtraction(client){

    const keyPhrasesInput = [
        "My cat might need to see a veterinarian.",
    ]

    const result = await client.extractKeyPhrases(keyPhrasesInput)


    result.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Key Phrases: ${document.keyPhrases}`);
    });
}
keyPhraseExtraction(textAnalyticsClient);
```

Kodunuzu konsol pencerenizde `node index.js` çalıştırın.

### <a name="output"></a>Çıktı

```console
ID: 0
        Document Key Phrases: cat,veterinarian
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı hızlı başlangıç dosyanızdaki `node` komutuyla çalıştırın.

```console
node index.js
```
