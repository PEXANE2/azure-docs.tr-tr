---
title: GenerateAnswer API 'SI ile meta veriler-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, anahtar/değer çiftleri biçiminde meta verileri soru/yanıt kümelerine eklemenize olanak tanır. Sonuçları Kullanıcı sorgularıyla filtreleyebilir ve izleme konuşmalarında kullanılabilecek ek bilgileri saklayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: diberry
ms.openlocfilehash: 2f9b624ffcc04963046ad817bb2bc9c025161506
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300263"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>GenerateAnswer API ve meta verileri ile bir yanıt alın

Kullanıcının sorusunun tahmin edilen yanıtını almak için GenerateAnswer API 'sini kullanın. Bir Bilgi Bankası yayımladığınızda, **Yayımlama** SAYFASıNDA bu API 'nin nasıl kullanılacağına ilişkin bilgileri görebilirsiniz. API 'yi, meta veri etiketlerine göre yanıtları filtrelemek için de yapılandırabilir ve test sorgu dizesi parametresiyle uç noktadan Bilgi Bankası ' nı test edebilirsiniz.

Soru-Cevap Oluşturma, anahtar ve değer çiftleri biçiminde meta verileri, soru ve yanıt kümelerinize eklemenizi sağlar. Daha sonra bu bilgileri Kullanıcı sorgularıyla sonuçları filtrelemek ve izleme konuşmalarında kullanılabilecek ek bilgileri depolamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Bilgi Bankası](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Soru ve yanıtları bir QnA varlığıyla depolayın

Soru-Cevap Oluşturma soruyu nasıl depolayacağınızı ve verileri nasıl yanıtlayabileceğinizi anlamak önemlidir. Aşağıdaki çizim bir QnA varlığını göstermektedir:

![Bir QnA varlığının çizimi](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her QnA varlığının benzersiz ve kalıcı bir KIMLIĞI vardır. Belirli bir QnA varlığında güncelleştirme yapmak için KIMLIĞINI kullanabilirsiniz.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>GenerateAnswer API 'SI ile yanıt tahminlerini alın

Soru ve yanıt kümelerinden en iyi eşleşmeyi elde etmek için, bot veya uygulamanızdaki [Generateanswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) 'sini bir Kullanıcı sorusu ile sorgulamak için kullanırsınız.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>GenerateAnswer uç noktasını almak için Yayımla 

Bilgi bankanızı [soru-cevap oluşturma portalından](https://www.qnamaker.ai)yayımladığınızda veya [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish)'Yi kullanarak, generateanswer uç noktanızın ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarınızı almak için:
1. [https://www.qnamaker.ai](https://www.qnamaker.ai) adresinde oturum açın.
1. **Bilgi tabanlarım**' da bilgi tabanınız Için **kodu görüntüle** ' yi seçin.
    ![Bilgi temellerimin ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. GenerateAnswer uç noktası ayrıntılarınızı alın.

    ![Uç nokta ayrıntılarının ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, bilgi Bankalarınızın **Ayarlar** sekmesinden uç nokta ayrıntılarınızı alabilirsiniz.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer istek yapılandırması

HTTP POST isteğiyle GenerateAnswer öğesini çağırın. GenerateAnswer çağrısının nasıl çağrılacağını gösteren örnek kod için bkz. [hızlı](../quickstarts/create-publish-kb-csharp-sdk.md#generate-an-answer-from-the-knowledge-base)başlangıçlara bakın. 

POST isteği şunu kullanır:

* Gerekli [URI parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#uri-parameters)
* Güvenlik için gerekli [üst bilgi özelliği](https://docs.microsoft.com/azure/cognitive-services/qnamaker/quickstarts/get-answer-from-knowledge-base-nodejs#add-a-post-request-to-send-question-and-get-an-answer) `Authorization`
* Gerekli [gövde özellikleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#feedbackrecorddto). 

GenerateAnswer URL 'SI aşağıdaki biçime sahiptir: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

Öğesinin `Authorization` http üst bilgisi özelliğini, sonundaki boşluk ve sonra **Ayarlar** sayfasında bulunan uç `EndpointKey` nokta anahtarı ile birlikte dizenin bir değeriyle ayarlamayı unutmayın.

Örnek bir JSON gövdesi şöyle görünür:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>GenerateAnswer yanıt özellikleri

[Yanıt](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#successful-query) , varsa yanıtı göstermek için ihtiyacınız olan tüm bilgileri ve bir sonraki konuşmayı IÇEREN bir JSON nesnesidir.

```json
{
    "answers": [
        {
            "score": 28.54820341616869,
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

## <a name="use-qna-maker-with-a-bot-in-c"></a>İçindeki bir bot ile Soru-Cevap Oluşturma kullanmaC#

Bot Framework, Soru-Cevap Oluşturma özelliklerine erişim sağlar:

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

Destek bot, bu koda [bir örnek](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-support/csharp_dotnetcore/Service/SupportBotService.cs#L418) içerir.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Node. js ' de bir bot ile Soru-Cevap Oluşturma kullanma

Bot Framework, Soru-Cevap Oluşturma özelliklerine erişim sağlar:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.03,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

Destek bot, bu koda [bir örnek](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs/Helpers/dialogHelper.js#L36) içerir.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Özel meta veri etiketlerine göre yanıtları filtrelemek için meta verileri kullanma

Meta veri eklemek, yanıtları bu meta veri etiketlerine göre filtrelemenize izin verir. **Görünüm seçenekleri** menüsünden meta veri sütununu ekleyin. Meta veri **+** simgesini seçerek, meta veri çifti eklemek için bilgi bankasından meta veriler ekleyin. Bu çift bir anahtar ve bir değer içerir.

![Meta veri ekleme ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Meta veri etiketleri için en strictFilters ile Sonuçları filtrele

"Bu otel ne zaman kapatıldığında?" Kullanıcı sorusunu göz önünde bulundurun.

Sonuçlar yalnızca Restoran "PARADISE" için gerekli olduğundan, "restoran adı" meta verilerinde GenerateAnswer çağrısında bir filtre ayarlayabilirsiniz. Aşağıdaki örnek şunu gösterir:

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

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Konuşma bağlamını tutmak için soru ve yanıt sonuçlarını kullanın

GenerateAnswer yanıtı, eşleşen soru ve yanıt kümesi için karşılık gelen meta veri bilgilerini içerir. Bu bilgileri, daha sonraki konuşmalarda kullanılmak üzere önceki görüşmenin bağlamını depolamak için istemci uygulamanızda kullanabilirsiniz. 

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

## <a name="match-questions-only-by-text"></a>Yalnızca soruları Eşleştir, metne göre

Varsayılan olarak, Soru-Cevap Oluşturma sorular ve yanıtlar arasında arama yapar. Yalnızca sorulardan arama yapmak istiyorsanız, yanıt oluşturmak için generateanswer isteğinin gönderi gövdesinde öğesini `RankerType=QuestionOnly` kullanın.

`isTest=false` Kullanarak`isTest=true`, veya kullanarak, ile yayımlanan KB veya test KB içinde arama yapabilirsiniz.

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="common-http-errors"></a>Ortak HTTP hataları

|Kod|Açıklama|
|:--|--|
|2xx|Başarılı|
|400|isteğin parametreleri yanlış gerekli parametreler eksik, hatalı biçimlendirilmiş ya da çok büyük olduğu anlamına gelir|
|400|isteğin gövde JSON eksik, hatalı biçimlendirilmiş ya da çok büyük yani yanlış|
|401|Geçersiz anahtar|
|403|Yasak - sizin doğru izinlere sahip değilsiniz|
|404|KB yok|
|410|Bu API kullanım dışı ve artık kullanılamıyor|

## <a name="next-steps"></a>Sonraki adımlar

**Yayımla** sayfası, [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) ve [kıvrımlı](../Quickstarts/get-answer-from-kb-using-curl.md)bir yanıt oluşturmak için bilgi de sağlar. 

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./create-knowledge-base.md)
