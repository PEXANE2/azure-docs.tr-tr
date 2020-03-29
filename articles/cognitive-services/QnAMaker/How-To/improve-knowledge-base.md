---
title: Bilgi tabanını geliştirin - QnA Maker
description: Aktif öğrenme ile bilgi tabanınızın kalitesini artırın. Varolan soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 8b2176731ce456b70521cc9208435d0b19943ab8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053077"
---
# <a name="accept-active-learning-suggested-questions-in-the-knowledge-base"></a>Bilgi tabanında aktif öğrenme önerilen soruları kabul edin


<a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>

Etkin Öğrenme, öneriyi onayladıktan sonra Bilgi Bankası'nı veya Arama Hizmetini değiştirir, ardından kaydedin ve eğitir. Öneriyi onaylarsanız, alternatif bir soru olarak eklenir.

## <a name="turn-on-active-learning"></a>Etkin öğrenmeyi açma

Önerilen soruları görmek için QnA Maker kaynağınız için [etkin öğrenmeyi açmanız](use-active-learning.md) gerekir.

## <a name="view-suggested-questions"></a>Önerilen soruları görüntüleme

1. Önerilen soruları görmek için bilgi tabanını **düzenle** **sayfasında, Seçenekleri Görüntüle'yi**seçin ve ardından **etkin öğrenme önerilerini göster'i**seçin.

    [![Portalın Düzenle bölümünde, etkin öğrenmenin yeni soru alternatiflerini görmek için Önerileri Göster'i seçin.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. **Önerilere Göre Filtre'yi**seçerek yalnızca önerileri göstermek için bilgi tabanını soru ve yanıt çiftleri ile filtreleyin.

    [![Yalnızca etkin öğrenmenin önerilen soru alternatiflerini görüntülemek için önerilerin geçişini uygulayın.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Her QnA çifti, soruyu kabul `✔` `x` etmek veya önerileri reddetmek için bir onay işareti ile yeni soru alternatifleri önerir. Soruyu eklemek için onay işaretini seçin.

    [![Yeşil onay işaretini veya kırmızı silme işaretini seçerek etkin öğrenmenin önerilen soru alternatiflerini seçin veya reddedin.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    **Tümünü Ekle** veya **Tümünü Tümünü** Reddet'i seçerek _tüm önerileri_ ekleyebilir veya silebilirsiniz.

1. Bilgi tabanındaki değişiklikleri kaydetmek için **Kaydet ve Eğit'i** seçin.

1. Değişikliklerin [GenerateAnswer API'sinden](metadata-generateanswer-usage.md#generateanswer-request-configuration)kullanılabilmesine izin vermek için **Yayımla'yı** seçin.

    Her 30 dakikada bir 5 veya daha fazla benzer sorgu kümelendiğinde, QnA Maker kabul veya reddetmeniz için alternatif soruları önerir.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Bir bottan GenerateAnswer ve Train API'lerini kullanmak için mimari akış

Bir bot veya başka bir istemci uygulaması, etkin öğrenmeyi kullanmak için aşağıdaki mimari akışı kullanmalıdır:

* Bot, bir dizi yanıt almak için `top` özelliği kullanarak GenerateAnswer API ile bilgi [tabanından yanıt alır.](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)
* Bot açık geribildirim belirler:
    * Kendi [özel iş mantığınızı](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user)kullanarak, düşük puanları filtreleyin.
    * Bot veya istemci uygulamasında, kullanıcıya olası yanıtların listesini görüntüleyin ve kullanıcının seçili yanıtını alın.
* Bot, seçilen yanıtı [Tren API'si](#train-api)ile [QnA Maker'a geri gönderir.](#bot-framework-sample-code)


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Birkaç eşleşen yanıt almak için GenerateAnswer isteğindeki en üst özelliği kullanma

Bir yanıt için QnA Maker'a soru `top` gönderirken, JSON gövdesinin özelliği dönmek için yanıt sayısını ayarlar.

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Kullanıcıya göstermek için yanıt listesini almak için iş mantığıyla birlikte puan özelliğini kullanın

İstemci uygulaması (sohbet botu gibi) yanıtı aldığında, en iyi 3 soru döndürülür. Puanlar `score` arasındaki yakınlığı analiz etmek için özelliği kullanın. Bu yakınlık aralığı kendi iş mantığı tarafından belirlenir.

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ *+ ' Wi-Fi Direct is displayed in the Status bar.",
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

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Sorular benzer puanlar olduğunda müşteri başvuru takibi

İstemci uygulamanız, kullanıcının niyetini en çok temsil eden _tek soruyu_ seçmesi seçeneğiyle soruları görüntüler.

Kullanıcı varolan sorulardan birini seçtikten sonra, istemci uygulaması QnA Maker's Train API'sini kullanarak kullanıcının seçimini geri bildirim olarak gönderir. Bu geri bildirim, etkin öğrenme geri bildirim döngüsütamamlar.

## <a name="train-api"></a>API eğitme

Aktif öğrenme geri bildirimi Tren API POST isteği ile QnA Maker'a gönderilir. API imzası:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP isteği özelliği|Adı|Tür|Amaç|
|--|--|--|--|
|URL rota parametresi|Bilgi bankası kimliği|string|Bilgi tabanınız için GUID.|
|Özel alt etki alanı|QnAMaker kaynak adı|string|Kaynak adı QnA Maker için özel alt etki alanı olarak kullanılır. Bu, bilgi tabanını yayımladıktan sonra Ayarlar sayfasında mevcuttur. Bu olarak listelenir `host`.|
|Üst bilgi|İçerik Türü|string|API'ye gönderilen gövdenin ortam türü. Varsayılan değer:`application/json`|
|Üst bilgi|Yetkilendirme|string|Uç nokta anahtarınız (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Post Body|JSON nesnesi|JSON|Eğitim geri bildirimi|

JSON gövdesinin çeşitli ayarları vardır:

|JSON vücut özelliği|Tür|Amaç|
|--|--|--|--|
|`feedbackRecords`|array|Geri bildirim listesi.|
|`userId`|string|Önerilen soruları kabul eden kişinin kullanıcı kimliği. Kullanıcı kimliği biçimi size kalmış. Örneğin, bir e-posta adresi mimarinizde geçerli bir kullanıcı kimliği olabilir. İsteğe bağlı.|
|`userQuestion`|string|Kullanıcının sorgusunun tam metni. Gereklidir.|
|`qnaID`|number|Sorunun kimliği, [GenerateAnswer yanıtında](metadata-generateanswer-usage.md#generateanswer-response-properties)bulunur. |

Bir örnek JSON vücut gibi görünüyor:

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

Başarılı bir yanıt 204 durumu döndürür ve hiçbir JSON yanıt gövdesi.

### <a name="batch-many-feedback-records-into-a-single-call"></a>Birçok geri bildirim kaydını tek bir aramada toplu olarak

Bir bot gibi istemci tarafı uygulamasında, verileri depolayabilir, ardından `feedbackRecords` dizide tek bir JSON gövdesinde birçok kayıt gönderebilirsiniz.

Bir örnek JSON vücut gibi görünüyor:

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

## <a name="bot-framework-sample-code"></a>Bot çerçeve örnek kodu

Kullanıcı sorgusu etkin öğrenme için kullanılacaksa, bot çerçeve kodunun Tren API'sini araması gerekir. Yazılması gereken iki kod vardır:

* Sorgunun etkin öğrenme için kullanılıp kullanılmaması gerektiğini belirleme
* Etkin öğrenme için sorguyı QnA Maker's Train API'sine geri gönderme

Azure [Bot örneğinde,](https://aka.ms/activelearningsamplebot)bu etkinliklerin her ikisi de programlanmıştır.

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Bot Framework 4.x ile Tren API için Örnek C# kodu

Aşağıdaki kod, Tren API'si ile QnA Maker'a nasıl bilgi gönderilebildiğini göstermektedir. Bu [tam kod örneği](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) GitHub'da mevcuttur.

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

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Bot Framework 4.x ile Tren API için Örnek Düğüm.js kodu

Aşağıdaki kod, Tren API'si ile QnA Maker'a nasıl bilgi gönderilebildiğini göstermektedir. Bu [tam kod örneği](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) GitHub'da mevcuttur.

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

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Aktif öğrenme ihraç bilgi tabanında kaydedilir

Uygulamanız etkin öğrenme etkinleştirildiğinde ve uygulamayı dışa aktardığınızda, tsv dosyasındaki `SuggestedQuestions` sütun etkin öğrenme verilerini korur.

Sütun, `SuggestedQuestions` örtük `autosuggested`ve açık, `usersuggested` geribildirim bilgilerinin bir JSON nesnesidir. Tek bir kullanıcı tarafından gönderilen soru için bu `help` JSON nesnesinin bir örneği:

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

Bu değişiklikleri gözden geçirmek için indirme değişiklikleri API'yi REST'i veya dil tabanlı SDK'lardan herhangi birini kullanarak da kullanabilirsiniz:
* [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


Bu uygulamayı yeniden içe aktardığınızda, etkin öğrenme bilgi toplamaya ve bilgi tabanınız için öneriler önermeye devam eder.



## <a name="best-practices"></a>En iyi uygulamalar

Etkin öğrenmeyi kullanırken en iyi uygulamalar için [en iyi uygulamalara](../Concepts/best-practices.md#active-learning)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GenerateAnswer API'si ile meta verileri kullanma](metadata-generateanswer-usage.md)
