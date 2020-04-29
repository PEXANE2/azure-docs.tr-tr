---
title: 'Öğretici: normal ifade varlığı-LUSıS'
description: Normal Ifade varlığını kullanarak, düzenli olarak biçimlendirilen verileri bir utterden ayıklayın.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: 5b585ee52880c474d3f2736b34a267012b390aad
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80545828"
---
# <a name="tutorial-get-well-formatted-data-from-the-utterance"></a>Öğretici: söylenişi 'ten iyi biçimlendirilmiş veriler alın
Bu öğreticide, düzenli olarak biçimlendirilen verileri bir utterde çıkarmak için bir normal ifade varlığı oluşturun.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Uygulamayı içeri aktar
> * Amaç ekleme
> * Normal ifade varlığı ekleme
> * Ayıklanan verileri almak için uygulamayı eğitme, yayımlama ve sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="regular-expression-entities"></a>Normal ifade varlıkları

Düzenli ifade varlığı kullanarak bir noktadan iyi biçimlendirilmiş metin çekin. Konuşmanın amacı her zaman makine öğrenimi ile belirlenirse de bu özel varlık türü makine öğrenimli değildir. Normal ifade varlığı için iyi bir kullanım, [düzenli bir ifadeyle](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)sürekli olarak gösterilebilen bir metindir.

`Send pizza delivery time to x123432`

Bu örnek, SMS mesajları göndermek için _kısa bir kod_ kullanır. Bu kısa kod, 5 veya 6 basamaklı bir sayısal koddur, bir x önekli ve normal ifadeyle `x\d{5,6}`açıklanabilir.

Bir LUSıS uygulamasına normal ifade varlığı eklediğinizde, metni normal Express varlığıyla [etiketlemenize](label-entity-example-utterance.md) gerek yoktur. Tüm amaçlar için tüm söyleye uygulanır.

## <a name="import-example-json-to-begin-app"></a>Uygulamayı başlatmak için example. JSON al

1.  [Uygulama json dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)indirip kaydedin.

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="create-intent-for-sending-confirmation-text-messages"></a>Onay metin iletileri göndermek için amaç oluştur

1. Bir onay metni göndermek için bir deterance 'in amacını sınıflandırmak üzere yeni bir amaç oluşturmak için **+ Oluştur** ' u seçin.

1. Açılan iletişim kutusuna `ConfirmationText` girip **Done** (Bitti) öğesini seçin.

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |X123432 'e pizza teslim süresi gönder|
    |X234567 için txt|
    |bildirim için x23987|

    Makine tarafından öğrenilen varlıkları ayıklamak için, varlığı farklı çeşitçlerle birlikte içeren örnekler sağlamanız gerekir, ancak bu makine tarafından öğrenilmemiş varlık, çeşitleme önemli değildir. Metin normal ifadeyle eşleştiği sürece ayıklanacaktır.

## <a name="use-the-regular-expression-entity-for-well-formatted-data"></a>İyi biçimlendirilmiş veriler için normal ifade varlığını kullanın
Metin numarasıyla eşleşecek bir normal ifade varlığı oluşturun. Bu normal ifade metinle eşleşiyor, ancak Case ve Culture türevlerini yoksayar.

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

1. Varlıklar listesi sayfasında **+ Oluştur** ' u seçin.

1. Açılan iletişim kutusunda yeni varlık adını `ConfirmationTextRegEx`girin, varlık türü olarak **Regex** ' ı seçin ve ardından **İleri**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Normal ifade varlığı için varlık oluşturma adımlarını Başlat](./media/luis-quickstart-intents-regex-entity/pizza-create-new-entity.png)

1. **Regex varlığı oluştur**' da, **Regex** değeri `x\d{5,6}` olarak girin ve ardından **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Örnek dıklarından verileri ayıklamak için normal ifade girin](./media/luis-quickstart-intents-regex-entity/pizza-set-regular-expression-for-new-entity.png)

1. Sol menüden **amaçlar** ' ı seçin, ardından **ConfirmationText** amacını görürsünüz.

    > [!div class="mx-imgBorder"]
    > ![Örnek söylerdeki etiketli normal ifadeyi görüntüle](./media/luis-quickstart-intents-regex-entity/pizza-reg-ex-entity-shown-example-utterances-intent.png)

    Varlık, makine tarafından öğrenilen bir varlık olmadığından, varlık, oluşturma işlemi tamamlandıktan hemen sonra LUO portalında görüntülenir.

## <a name="train-the-app-before-testing-or-publishing"></a>Test veya yayımlamadan önce uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktadan amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ ile değiştirin:

    `Text my pizza delivery to x23456 x234567 x12345`

    ```json
    {
        "query": "Text my pizza delivery to x23456 x234567 x12345",
        "prediction": {
            "topIntent": "ConfirmationText",
            "intents": {
                "ConfirmationText": {
                    "score": 0.7061845
                },
                "ModifyOrder": {
                    "score": 0.196021989
                },
                "None": {
                    "score": 0.02342912
                },
                "Test-Pizza": {
                    "score": 0.01213586
                },
                "CancelOrder": {
                    "score": 0.0063042324
                },
                "Confirmation": {
                    "score": 0.0058615827
                },
                "Greetings": {
                    "score": 0.00398947531
                }
            },
            "entities": {
                "ConfirmationTextRegEx": [
                    "x23456",
                    "x234567",
                    "x12345"
                ],
                "$instance": {
                    "ConfirmationTextRegEx": [
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x23456",
                            "startIndex": 26,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x234567",
                            "startIndex": 33,
                            "length": 7,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x12345",
                            "startIndex": 41,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
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

    LUIS, normal ifade varlığı kullanarak adlandırılmış verileri ayıklar ve bu durum, JSON yanıtını alan istemci yazılımı için program açısından daha faydalıdır.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Kavram-varlıklar](luis-concept-entity-types.md)
* [Normal ifade varlığının JSON başvurusu](reference-entity-regular-expression.md?tabs=V3)
* [Verileri ayıklamak için varlık ekleme](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu, örnek konuşmalar eklendi ve ardından konuşmalardan düzgün biçimlendirilmiş veriler ayıklamak için normal ifade varlığı eklendi. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Liste varlığı hakkında bilgi edinin](tutorial-list-entity.md)

