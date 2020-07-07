---
title: 'Öğretici: uç nokta utterlerini gözden geçirme-LUSıS'
description: Bu öğreticide, LUSıS 'nin emin olduğu LUSıS HTTP uç noktası aracılığıyla alınan utslarını doğrulayarak veya düzelterek uygulama tahminlerini geliştirebilirsiniz. Bazı konuşmaların amacının, diğerlerinin ise varlığının doğrulanması gerekebilir.
services: cognitive-services
ms.topic: tutorial
ms.date: 07/02/2020
ms.openlocfilehash: 082e625efeeb4764aaa1ac5101eb2b0013348b19
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959063"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>Öğretici: uç nokta dıklarını inceleyerek, hariç tahminleri çözme
Bu öğreticide, LUSıS HTTPS uç noktası aracılığıyla alınan ve bu LUSıS 'in eksik olduğu noktaları doğrulayarak veya düzelterek uygulama tahminlerini geliştirebilirsiniz. Zamanlanan LUSıS bakımın düzenli bir parçası olarak uç nokta utslerini incelemeniz gerekir.

Bu gözden geçirme işlemi, LUSıS 'in uygulama etki alanınızı öğrenmeye olanak tanır. LUO, gözden geçirme listesinde görünen detersliği seçer. Bu liste:

* Uygulamaya özeldir.
* Uygulamanın tahmin doğruluğunu geliştirmek için hazırlanmıştır.
* Düzenli aralıklarla gözden geçirilmelidir.

Uç nokta ifadelerini gözden geçirerek, ifadenin tahmin edilen amacını doğrular veya düzeltirsiniz.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Uç nokta ifadelerini gözden geçirme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="download-json-file-for-app"></a>Uygulama için JSON dosyasını indirin

[Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/tutorial-fix-unsure-predictions.json?raw=true) indirip kaydedin.

## <a name="import-json-file-for-app"></a>Uygulama için JSON dosyasını içeri aktar


1. [Luu portalında](https://www.luis.ai), **uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktarın**. Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti** 'yi seçin

1. Bir LUSıS uygulamasının ana bina blokları olan amaçları görmek için **Oluştur** ve **amaçlar** ' ı seçin.

    :::image type="content" source="media/luis-tutorial-review-endpoint-utterances/initial-intents-in-app.png" alt-text="Sürümler sayfasından amaçlar sayfasına geçin.":::

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Uygulamaya varlık değişikliklerini uygulamak için uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Uygulamayı HTTP uç noktasından erişmek üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="add-utterances-at-the-endpoint"></a>Uç noktada utsliği ekleme

Bu uygulamada, hedefleri ve varlıkları vardır ancak uç nokta kullanımınız yoktur. Bu uç nokta kullanımı, uç nokta söylenişi gözden geçirimiyle uygulamayı geliştirmek için gereklidir.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ aşağıdaki tabloda yer alacak şekilde değiştirin. Her söylük için, utterance 'i iletin ve sonucu elde edin. Ardından, sonundaki söylik 'i sonraki söyleyle değiştirin.

    |Uç nokta söylenişi|Hizalanmış amaç|
    |--|--|
    |`I'm looking for a job with Natural Language Processing`|`GetJobInformation`|
    |`I want to cancel on March 3`|`Utilities.Cancel`|
    |`When were HRF-123456 and hrf-234567 published in the last year?`|`FindForm`|
    |`shift 123-45-6789 from Z-1242 to T-54672`|`MoveEmployee`|
    |`Please relocation jill-jones@mycompany.com from x-2345 to g-23456`|`MoveEmployee`|
    |`Here is my c.v. for the programmer job`|`ApplyForJob`|
    |`This is the lead welder paperwork.`|`ApplyForJob`|
    |`does form hrf-123456 cover the new dental benefits and medical plan`|`FindForm`|
    |`Jill Jones work with the media team on the public portal was amazing`|`EmployeeFeedback`|

    Hangi sürümü etkin olarak düzenlemekte olduğunuzdan veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak gözden geçirilecek tek bir konuşma havuzu vardır.

## <a name="review-endpoint-utterances"></a>Uç nokta ifadelerini gözden geçirme

Doğru hizalanmış amaç için uç nokta utslerini gözden geçirin. Tüm sürümlerde gözden geçirilecek tek bir havuz havuzu olsa da, amacı doğru şekilde hizalamak, örneği yalnızca geçerli _etkin modele_ ekler.

1. Portalın **Build** bölümünde sol gezinmede **uç nokta Utlerini gözden geçir** ' i seçin. Bu liste **ApplyForJob** amacı için filtrelenmiştir.

    :::image type="content" source="./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png" alt-text="Sol gezinti aşamasında uç nokta utsliği düğmesinin ekran görüntüsü.":::

    Bu utterance, `I'm looking for a job with Natural Language Processing` doğru amaç, _Getjobınformation_içinde değil. İki amaç içindeki iş adları ve fiillerin benzerliği nedeniyle _Applyforjob_ olarak tahmin edildi.

1.  Bu söyleyeni hizalamak için doğru **hizalanmış hedefini** seçin `GetJobInformation` . Onay işaretini seçerek değiştirilen söylenişi 'i uygulamaya ekleyin.

    Bu amaca göre kalan diğer çizgileri inceleyerek, hizalanmış amacı gereken şekilde düzeltin. Hizalanmış amacı görüntülemek için bu öğreticideki ilk söylenişi tablosunu kullanın.

    **Gözden geçirme uç noktası uttersları** listesi artık düzeltilen noktalara sahip olmamalıdır. Daha fazla yer görünürse, liste boş olana kadar hizalanmış hedefleri düzelterek listede çalışmaya devam edin.

    Varlık etiketlemenin herhangi bir düzeltmesi, amaç ayrıntıları sayfasından, amaç hizalandıktan sonra yapılır.

1. Uygulamayı yeniden eğitin ve yayımlayın.

## <a name="get-intent-prediction-from-endpoint"></a>Uç noktadan amaç tahminini al

Doğru hizalanmış örnek, uygulamanın tahmininin iyileştirildiklerini doğrulamak için, düzeltilen söylenişi 'e yakın bir şekilde deneyin.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ ile değiştirin `Are there any natural language processing jobs in my department right now?` .

   ```json
    {
        "query": "Are there any natural language processing jobs in my department right now?",
        "prediction": {
            "topIntent": "GetJobInformation",
            "intents": {
                "GetJobInformation": {
                    "score": 0.901367366
                },
                "ApplyForJob": {
                    "score": 0.0307973567
                },
                "EmployeeFeedback": {
                    "score": 0.0296942145
                },
                "MoveEmployee": {
                    "score": 0.00739785144
                },
                "FindForm": {
                    "score": 0.00449316856
                },
                "Utilities.Stop": {
                    "score": 0.00417657848
                },
                "Utilities.StartOver": {
                    "score": 0.00407167152
                },
                "Utilities.Help": {
                    "score": 0.003662492
                },
                "None": {
                    "score": 0.00335733569
                },
                "Utilities.Cancel": {
                    "score": 0.002225436
                },
                "Utilities.Confirm": {
                    "score": 0.00107437756
                }
            },
            "entities": {
                "keyPhrase": [
                    "natural language processing jobs",
                    "department"
                ],
                "datetimeV2": [
                    {
                        "type": "datetime",
                        "values": [
                            {
                                "timex": "PRESENT_REF",
                                "resolution": [
                                    {
                                        "value": "2020-07-02 21:45:50"
                                    }
                                ]
                            }
                        ]
                    }
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "natural language processing jobs",
                            "startIndex": 14,
                            "length": 32,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "builtin.keyPhrase",
                            "text": "department",
                            "startIndex": 53,
                            "length": 10,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "datetimeV2": [
                        {
                            "type": "builtin.datetimeV2.datetime",
                            "text": "right now",
                            "startIndex": 64,
                            "length": 9,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
   ```

   Artık eksik olan söyleymelerin doğru hizalandığına göre, doğru amaç **yüksek puan**ile tahmin edildi.

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Gözden geçirme işleminin yerini daha fazla ifade ekleme alabilir mi?
Neden daha fazla örnek ifade eklenmediğini merak ediyor olabilirsiniz. Uç nokta ifadelerini gözden geçirmenin amacı nedir? Gerçek dünyadaki bir LUIS uygulamasında, uç nokta ifadeleri henüz alışmadığınız sözcük seçimi ve yerleşimi olan kullanıcılara aittir. Aynı sözcük seçimini ve yerleşimini daha önce kullandıysanız, özgün tahminin yüzdesi yüksek olabilir.

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>En üst amaç neden ifade listesinde yer alıyor?
Bazı uç nokta konuşmalarının gözden geçirme listesinde yüksek bir tahmin puanı olacaktır. Bu ifadeleri yine de gözden geçirmeniz ve doğrulamanız gerekir. Bunların listede olmasının nedeni, bir sonraki yüksek amacın puanının en üst amaç puanına yakın olmasıdır. İlk iki amaç arasında yaklaşık %15 bir fark olmasını istersiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide uç noktada gönderilen LUIS'in emin olmadığı konuşmaları incelediniz. Bu konuşmalar doğrulandıktan ve örnek konuşmalar olarak doğru amaçlara taşındıktan sonra LUIS tahmin etme doğruluğunu artıracaktır.

> [!div class="nextstepaction"]
> [Desenleri kullanmayı öğrenin](luis-tutorial-pattern.md)
