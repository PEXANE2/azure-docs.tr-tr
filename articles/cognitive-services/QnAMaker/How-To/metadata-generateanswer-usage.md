---
title: GenerateAnswer API ile meta veriler - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker, soru/yanıt kümelerinize anahtar/değer çiftleri şeklinde meta veriler eklemenize olanak tanır. Sonuçları kullanıcı sorgularına filtreleyebilir ve izleme konuşmalarında kullanılabilecek ek bilgileri depolayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: diberry
ms.openlocfilehash: 9beb6dbbba1c5855b8bfa97fc02f50aa59225d78
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474859"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>GenerateAnswer API ve meta verilerle yanıt alın

Bir kullanıcının sorusunun öngörülen yanıtını almak için GenerateAnswer API'sini kullanın. Bir bilgi tabanı yayımladığınızda, bu API'nin Nasıl kullanılacağıyla ilgili bilgileri **Yayımla** sayfasında görebilirsiniz. Ayrıca, api'yi yanıtları meta veri etiketlerine göre filtrelemek için yapılandırabilir ve test sorgusu dize parametresi ile bitiş noktasından bilgi tabanını sınayabilirsiniz.

QnA Maker, soru ve yanıt kümelerinize anahtar ve değer çiftleri şeklinde meta veriler eklemenize olanak tanır. Daha sonra bu bilgileri, sonuçları kullanıcı sorgularına filtrelemek ve izleme konuşmalarında kullanılabilecek ek bilgileri depolamak için kullanabilirsiniz. Daha fazla bilgi için [Bilgi tabanına](../Concepts/knowledge-base.md)bakın.

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Soruları ve yanıtları Bir QnA varlığıyla depolama

QnA Maker'ın soru ve yanıt verilerini nasıl depolayacağını anlamak önemlidir. Aşağıdaki resimde bir QnA varlığı gösterilmektedir:

![QnA varlığının çizimi](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her QnA varlığı benzersiz ve kalıcı bir kimliği vardır. Belirli bir QnA varlığında güncelleştirmeler yapmak için kimliği kullanabilirsiniz.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>GenerateAnswer API ile yanıt tahminleri alın

Soru ve yanıt kümelerinden en iyi eşleşmeyi elde etmek için bilgi tabanınızı bir kullanıcı sorusuyla sorgulamak için botunuzda veya uygulamanızda [GenerateAnswer API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) kullanırsınız.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>GenerateAnswer bitiş noktasını almak için yayımla

Bilgi tabanınızı, [QnA Maker portalından](https://www.qnamaker.ai)veya [API'yi](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish)kullanarak yayımladıktan sonra GenerateAnswer bitiş noktanızın ayrıntılarını alabilirsiniz.

Bitiş noktası bilgilerinizi almak için:
1. Oturum [https://www.qnamaker.ai](https://www.qnamaker.ai)aç.
1. **Bilgi tabanımda,** bilgi tabanınız için **Kodu Görüntüle'yi** seçin.
    ![Bilgi tabanlarımın ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. GenerateAnswer uç nokta bilgilerinizi alın.

    ![Uç nokta ayrıntılarının ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/view-code.png)

Bitiş noktası bilgilerinizi bilgi tabanınızın **Ayarlar** sekmesinden de alabilirsiniz.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer istek yapılandırması

Http POST isteğiyle GenerateAnswer'ı ararsınız. GenerateAnswer'ın nasıl çağrılacağı gösteren örnek kod için [hızlı başlangıçlara](../quickstarts/quickstart-sdk.md#generate-an-answer-from-the-knowledge-base)bakın.

POST isteği kullanır:

* Gerekli [URI parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#uri-parameters)
* Güvenlik için gerekli `Authorization`üstbilgi özelliği,
* Gerekli [vücut özellikleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#feedbackrecorddto).

GenerateAnswer URL'si aşağıdaki biçime sahiptir:

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

Bu başlık özelliğini, `Authorization` dize `EndpointKey` değeri ve ayarlar sayfasında bulunan bitiş noktası anahtarıyla **Settings** ayarlamayı unutmayın.

Bir örnek JSON vücut gibi görünüyor:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 30,
    "rankerType": "" // values: QuestionOnly
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

[RankerType](../concepts/best-practices.md#choosing-ranker-type)hakkında daha fazla bilgi edinin.

Önceki JSON yalnızca eşik puanının %30'u veya üzerinde olan yanıtlar istemiştir.

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>Yanıt oluşturma özellikleri

[Yanıt,](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#successful-query) yanıtı görüntülemek için gereken tüm bilgileri ve varsa konuşmadaki bir sonraki dönüşiçeren bir JSON nesnesidir.

```json
{
    "answers": [
        {
            "score": 38.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
            "metadata": [
                {
                    "name": "category",
                    "value": "api"
                }
            ]
        }
    ]
}
```

Önceki JSON% 38,5 puan ile bir cevap ile yanıt verdi.

## <a name="use-qna-maker-with-a-bot-in-c"></a>C bir bot ile QnA Maker kullanın #

Bot çerçevesi [getAnswer API](https://docs.microsoft.com/dotnet/api/microsoft.bot.builder.ai.qna.qnamaker.getanswersasync?view=botbuilder-dotnet-stable#Microsoft_Bot_Builder_AI_QnA_QnAMaker_GetAnswersAsync_Microsoft_Bot_Builder_ITurnContext_Microsoft_Bot_Builder_AI_QnA_QnAMakerOptions_System_Collections_Generic_Dictionary_System_String_System_String__System_Collections_Generic_Dictionary_System_String_System_Double__)ile QnA Maker özelliklerine erişim sağlar:

```csharp
using Microsoft.Bot.Builder.AI.QnA;
var metadata = new Microsoft.Bot.Builder.AI.QnA.Metadata();
var qnaOptions = new QnAMakerOptions();

metadata.Name = Constants.MetadataName.Intent;
metadata.Value = topIntent;
qnaOptions.StrictFilters = new Microsoft.Bot.Builder.AI.QnA.Metadata[] { metadata };
qnaOptions.Top = Constants.DefaultTop;
qnaOptions.ScoreThreshold = 0.3F;
var response = await _services.QnAServices[QnAMakerKey].GetAnswersAsync(turnContext, qnaOptions);
```

Önceki JSON yalnızca eşik puanının %30'u veya üzerinde olan yanıtlar istemiştir.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Node.js bir bot ile QnA Maker kullanın

Bot çerçevesi [getAnswer API](https://docs.microsoft.com/javascript/api/botbuilder-ai/qnamaker?view=botbuilder-ts-latest#generateanswer-string---undefined--number--number-)ile QnA Maker özelliklerine erişim sağlar:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.30,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

Önceki JSON yalnızca eşik puanının %30'u veya üzerinde olan yanıtlar istemiştir.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Yanıtları özel meta veri etiketlerine göre filtrelemek için meta verileri kullanma

Meta veri eklemek, yanıtları bu meta veri etiketlerine göre filtrelemenize olanak tanır. **Görünüm Seçenekleri** menüsünden meta veri sütunu ekleyin. Meta veri çifti eklemek için meta veri **+** simgesini seçerek bilgi tabanınıza meta veri ekleyin. Bu çift bir anahtar ve bir değerden oluşur.

![Meta veri ekleme ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Meta veri etiketleri için katı Filtreler ile filtre sonuçları

"Bu otel ne zaman kapanıyor?" sorusunu düşünün.

Sonuçlar yalnızca "Paradise" restoranı için gerekli olduğundan, "Restoran Adı" meta verilerinde GenerateAnswer çağrısında bir filtre ayarlayabilirsiniz. Aşağıdaki örnek bunu gösterir:

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

<a name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Konuşma bağlamını korumak için soru ve yanıt sonuçlarını kullanma

GenerateAnswer yanıtı eşleşen soru ve yanıt kümesinin karşılık gelen meta veri bilgilerini içerir. Bu bilgileri istemci uygulamanızda, önceki konuşmanın bağlamını sonraki konuşmalarda kullanılmak üzere depolamak için kullanabilirsiniz.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="match-questions-only-by-text"></a>Soruları yalnızca metinle eşleştirme

Varsayılan olarak, QnA Maker sorular ve yanıtlar aracılığıyla arama. Yalnızca sorular arasında arama yapmak, bir yanıt oluşturmak `RankerType=QuestionOnly` istiyorsanız, GenerateAnswer isteğinin POST gövdesini kullanın.

Yayınlanan kb'de, kullanarak `isTest=false`veya test kb'de `isTest=true`kullanarak arama yapabilirsiniz.

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="common-http-errors"></a>Sık kullanılan HTTP hataları

|Kod|Açıklama|
|:--|--|
|2xx|Başarılı|
|400|İstek parametreleri yanlış, yani gerekli parametreler eksik, biçimsiz veya çok büyük|
|400|İstek'in gövdesi yanlış, yani JSON eksik, biçimsiz veya çok büyük|
|401|Geçersiz anahtar|
|403|Yasak - doğru izinlerin yok|
|404|KB diye biri yok.|
|410|Bu API amortismana hazır ve artık kullanılamıyor|

## <a name="next-steps"></a>Sonraki adımlar

**Yayımla** sayfası ayrıca Postacı veya cURL ile [bir yanıt oluşturmak](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md) için bilgi sağlar.

> [!div class="nextstepaction"]
> [Bir bilgi bankası botu oluşturun](../tutorials/integrate-qnamaker-luis.md)
