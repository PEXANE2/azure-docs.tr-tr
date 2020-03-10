---
title: Bilgi Bankası Soru-Cevap Oluşturma geliştirme-
titleSuffix: Azure Cognitive Services
description: Etkin öğrenme ile bilgi Bankalarınızın kalitesini artırabilirsiniz. Mevcut soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin, ekleyin.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: diberry
ms.openlocfilehash: dea2bf3b34ca336f3932dd85bf587184ab6881db
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78389296"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Bilgi bankanızı geliştirmek için etkin öğrenmeyi kullanma

[Etkin öğrenme](../Concepts/active-learning-suggestions.md) , sorularınızı ve yanıt çiftiyle Kullanıcı Gönderimlerini temel alan alternatif sorular önererek bilgi Bankalarınızın kalitesini iyileştirmenize olanak tanır. Bu önerileri, mevcut sorulara ekleyerek veya reddetmek için gözden geçirin.

Bilgi tabanınız otomatik olarak değişmez. Değişikliklerin etkili olabilmesi için önerileri kabul etmelisiniz. Bu öneriler, sorular ekler ancak mevcut soruları değiştirmez veya kaldırmaz.


## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Çalışma zamanı sürümünüzü etkin öğrenimi kullanacak şekilde yükseltme

Etkin öğrenme, çalışma zamanı sürümü 4.4.0 ve üzeri sürümlerde desteklenir. Bilgi tabanınız daha önceki bir sürümde oluşturulduysa, [çalışma alanınızı](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) bu özelliği kullanacak şekilde yükseltin.

## <a name="turn-on-active-learning-to-see-suggestions"></a>Önerileri görmek için etkin öğrenmeyi etkinleştirin

Etkin öğrenme varsayılan olarak kapalıdır. Önerilen soruları görmek için etkinleştirin. Etkin öğrenmeyi etkinleştirdikten sonra, Soru-Cevap Oluşturma için istemci uygulamasından bilgi göndermeniz gerekir. Daha fazla bilgi için bkz. [GenerateAnswer kullanımı Için mimari akış ve bir bot 'Tan API 'Ler eğitme](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Bilgi Bankası 'nı yayımlamak için **Yayımla** ' yı seçin. Etkin öğrenme sorguları yalnızca GenerateAnswer API tahmin uç noktasından toplanır. Soru-Cevap Oluşturma portalındaki test bölmesi sorguları etkin öğrenmeyi etkilemez.

1. Soru-Cevap Oluşturma portalında etkin öğrenimi açmak için sağ üst köşeye gidin, **adınızı**seçin, [**hizmet ayarları**](https://www.qnamaker.ai/UserSettings)' na gidin.

    ![Etkin öğrenimi 'nin önerilen soru alternatiflerini hizmet ayarları sayfasından açın. Sağ üst menüdeki Kullanıcı adınızı seçip hizmet ayarları ' nı seçin.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Soru-Cevap Oluşturma hizmeti bulun ve ardından **etkin öğrenimi**değiştirin.

    > [!div class="mx-imgBorder"]
    > [Hizmet ayarları sayfasında ![, etkin öğrenme özelliği ' ne geçiş yapın. Özelliği geçiş yapadıysanız hizmetinizi yükseltmeniz gerekebilir.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > Önceki görüntüde yer alan tam sürüm yalnızca bir örnek olarak gösterilir. Sürümünüz farklı olabilir.

    **Etkin öğrenme** etkinleştirildikten sonra, Bilgi Bankası Kullanıcı tarafından gönderilen sorulara göre düzenli aralıklarla yeni sorular önerir. Ayarı yeniden değiştirerek **etkin öğrenmeyi** devre dışı bırakabilirsiniz.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Bilgi Bankası 'nda etkin bir öğrenme önerisi kabul etme

Etkin öğrenme, öneriyi onayladıktan sonra bilgi bankasını veya Arama Hizmeti değiştirir, ardından Kaydet ve eğitme. Öneriyi onayladıysanız, alternatif bir soru olarak eklenecektir.

1. Önerilen soruları görmek için Bilgi Bankası **Düzenle** sayfasında, **seçenekleri görüntüle**' yi seçin ve ardından **etkin öğrenme önerilerini göster**' i seçin.

    [Portalın düzenleme bölümünde ![, etkin öğrenmesinin yeni soru alternatiflerini görmek için önerileri göster ' i seçin.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Yalnızca önerilere **göre filtrele**' yi seçerek önerileri göstermek için bilgi bankasını soru ve yanıt çiftleri ile filtreleyin.

    [![önerilere göre filtrele ' ye tıklayarak yalnızca etkin öğrenme 'nın önerilen soru alternatiflerini görüntüleyin.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Her QnA çifti, bir onay işareti ile yeni soru alternatiflerini `✔`, soruyu kabul etmek için soru veya bir `x` kabul eder. Soruyu eklemek için onay işaretini seçin.

    [![yeşil onay işaretini veya kırmızı silme işaretini seçerek, etkin öğrenmesinin önerilen soru alternatiflerini seçin veya reddedin.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Bağlam araç çubuğunda Tümünü **Ekle** veya **Tümünü Reddet** ' i seçerek _tüm önerileri_ ekleyebilir veya silebilirsiniz.

1. Bilgi Bankası 'ndaki değişiklikleri kaydetmek için **Kaydet ve eğitme '** yi seçin.

1. Değişikliklerin [Generateanswer API](metadata-generateanswer-usage.md#generateanswer-request-configuration)'sinden kullanılabilir olmasını sağlamak için **Yayımla** ' yı seçin.

    5 veya daha fazla benzer sorgu kümelenirken, her 30 dakikada bir Soru-Cevap Oluşturma, kabul etmeniz veya reddetmeniz için alternatif soruları önerir.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Bir bot 'tan GenerateAnswer kullanımı ve API 'Leri eğitmek için mimari akış

Bir bot veya diğer istemci uygulaması, etkin öğrenimi kullanmak için aşağıdaki mimari akışı kullanmalıdır:

* Bot, [bilgi bankasındaki yanıtı](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) , `top` özelliğini kullanarak GENERATEANSWER API 'si ile bir dizi yanıt almak için alır.
* Bot açık geri bildirimi belirler:
    * Kendi [özel iş mantığınızı](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user)kullanarak düşük puanları filtreleyin.
    * Bot veya istemci uygulamasında, kullanıcıya olası yanıtların listesini görüntüleyin ve kullanıcının seçili yanıtını alın.
* Bot, seçili yanıtı, [tren API](#train-api)'siyle [soru-cevap oluşturma geri gönderir](#bot-framework-sample-code) .


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Birkaç eşleşen yanıt almak için GenerateAnswer isteğindeki top özelliğini kullanın

Yanıt için Soru-Cevap Oluşturma bir soru gönderilirken, JSON gövdesinin `top` özelliği döndürülecek yanıt sayısını ayarlar.

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Kullanıcı gösterme yanıtlarının listesini almak için, iş mantığı ile birlikte Score özelliğini kullanın

İstemci uygulaması (bir sohbet bot gibi) yanıtı aldığında, ilk 3 soru döndürülür. Puanlar arasındaki yakınlığı çözümlemek için `score` özelliğini kullanın. Bu yakınlık aralığı, kendi iş mantığınızla belirlenir.

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Soruların benzer puanları olduğu durumlarda istemci uygulaması izleme

İstemci uygulamanız, kullanıcının amaç temsil eden _tek soruyu_ seçmesini sağlayan bir seçenekle birlikte soruları görüntüler.

Kullanıcı mevcut sorulardan birini seçtiğinde, istemci uygulaması Soru-Cevap Oluşturma eğitim API 'sini kullanarak kullanıcının tercih ettiği geri bildirim olarak gönderir. Bu geri bildirim, etkin öğrenme geri bildirim döngüsünü tamamlar.

## <a name="train-api"></a>API eğitme

Etkin öğrenme geri bildirimi, API POST isteği ile Soru-Cevap Oluşturma gönderilir. API imzası:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP istek özelliği|Adı|Tür|Amaç|
|--|--|--|--|
|URL yol parametresi|Bilgi Bankası KIMLIĞI|string|Bilgi tabanınız için GUID.|
|Özel alt etki alanı|QnAMaker kaynak adı|string|Kaynak adı, Soru-Cevap Oluşturma özel alt etki alanı olarak kullanılır. Bu, Bilgi Bankası 'nı yayımladıktan sonra Ayarlar sayfasında bulunur. `host`olarak listelenir.|
|Üst bilgi|Content-Type|string|API 'ye gönderilen gövdenin medya türü. Varsayılan değer: `application/json`|
|Üst bilgi|Yetkilendirme|string|Uç nokta anahtarınız (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Gövde gönder|JSON nesnesi|JSON|Eğitim geri bildirimi|

JSON gövdesi çeşitli ayarlara sahiptir:

|JSON Body özelliği|Tür|Amaç|
|--|--|--|--|
|`feedbackRecords`|array|Geri bildirim listesi.|
|`userId`|string|Önerilen soruları kabul eden kişinin Kullanıcı KIMLIĞI. Kullanıcı KIMLIĞI biçimi size ait. Örneğin, bir e-posta adresi, mimarinizdeki geçerli bir kullanıcı KIMLIĞI olabilir. İsteğe bağlı.|
|`userQuestion`|string|Kullanıcının sorgusunun tam metni. Gereklidir.|
|`qnaID`|number|[Generateanswer yanıtında](metadata-generateanswer-usage.md#generateanswer-response-properties)bulunan soru kimliği. |

Örnek bir JSON gövdesi şöyle görünür:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Başarılı bir yanıt, 204 durumunu ve hiçbir JSON yanıt gövdesi döndürmez.

### <a name="batch-many-feedback-records-into-a-single-call"></a>Tek bir çağrıda toplu işlem birçok geri bildirim kaydı

Bir bot gibi istemci tarafı uygulamada, verileri depolayıp `feedbackRecords` dizisindeki tek bir JSON gövdesinde birçok kayıt gönderebilirsiniz.

Örnek bir JSON gövdesi şöyle görünür:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Bot Framework örnek kodu

Kullanıcı sorgusunun etkin öğrenme için kullanılması gerekiyorsa, bot Framework kodunuzun tren API 'sini çağırması gerekir. Yazılacak iki kod parçası vardır:

* Sorgunun etkin öğrenme için kullanılması gerekip gerekmediğini belirleme
* Sorguyu etkin öğrenme için Soru-Cevap Oluşturma eğitme API 'sine geri gönder

[Azure bot örneğinde](https://aka.ms/activelearningsamplebot), bu etkinliklerin her ikisi de programlanmıştır.

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Bot C# Framework 4. x Ile API eğitimi için örnek kod

Aşağıdaki kod, tren API 'siyle Soru-Cevap Oluşturma bilgilerin geri nasıl gönderileceğini gösterir. Bu [kod örneği](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) , GitHub ' da kullanılabilir.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="endpoint">Endpoint URI of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string endpoint, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = endpoint + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Bot Framework 4. x ile API eğitimi için örnek Node. js kodu

Aşağıdaki kod, tren API 'siyle Soru-Cevap Oluşturma bilgilerin geri nasıl gönderileceğini gösterir. Bu [kod örneği](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) , GitHub ' da kullanılabilir.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);

            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Etkin öğrenme, dışarıya kaydedilen Bilgi Bankası 'nda kaydedilir

Uygulamanızda etkin öğrenme etkin olduğunda ve uygulamayı dışarı aktardığınızda, TSV dosyasındaki `SuggestedQuestions` sütunu etkin öğrenme verilerini korur.

`SuggestedQuestions` sütunu örtük, `autosuggested`ve açık `usersuggested` geri bildirimlerinden oluşan bir JSON nesnesidir. `help` Kullanıcı tarafından gönderilen tek bir soru için bu JSON nesnesine bir örnek:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

REST veya dil tabanlı SDK 'Lardan birini kullanarak bu değişiklikleri gözden geçirmek için GERI yükleme değişiklikleri API 'sini de kullanabilirsiniz:
* [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


Bu uygulamayı yeniden içeri aktardığınızda, etkin öğrenme bilgileri toplamaya ve bilgi tabanınız için öneriler önermeye devam etmektedir.



## <a name="best-practices"></a>En iyi uygulamalar

Etkin öğrenimi kullanırken en iyi uygulamalar için bkz. [en iyi uygulamalar](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GenerateAnswer API 'SI ile meta verileri kullanma](metadata-generateanswer-usage.md)
