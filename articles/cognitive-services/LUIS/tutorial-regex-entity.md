---
title: 'Öğretici: Normal ifade varlığı - LUIS'
titleSuffix: Azure Cognitive Services
description: Düzenli İfade varlığını kullanarak bir sözcükten sürekli biçimlendirilmiş verileri ayıklayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: 0ca6f2a67e01e4c604c2dcc8f8eaa9ffe8bad045
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75381537"
---
# <a name="tutorial-get-well-formatted-data-from-the-utterance"></a>Öğretici: Söyleyişten iyi biçimlendirilmiş veriler alın
Bu öğreticide, bir sözcükten sürekli biçimlendirilmiş verileri ayıklamak için düzenli bir ifade varlığı oluşturun.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Uygulama alma
> * Amaç ekleme
> * Normal ifade varlığı ekleme
> * Çıkarılan verileri almak için uygulama, yayımlama ve sorgu uygulaması

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="regular-expression-entities"></a>Normal ifade varlıkları

Bir söyleyişten iyi biçimlendirilmiş metni çıkarmak için normal ifade varlığını kullanın. Konuşmanın amacı her zaman makine öğrenimi ile belirlenirse de bu özel varlık türü makine öğrenimli değildir. Normal ifade varlığı için iyi bir kullanım, düzenli bir [ifade](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)yle sürekli olarak temsil edilebilen herhangi bir metindir.

`Send pizza delivery time to x123432`

Bu örnek, metin iletileri göndermek için kısa bir _kod_ kullanır. Bu kısa kod, x ile önceden belirlenmiş 5 veya 6 basamaklı sayısal bir koddur `x\d{5,6}`ve normal ifade ile tanımlanabilir.

Bir LUIS uygulamasına normal bir ifade varlığı eklediğinizde, metni normal ifade varlığıyla [etiketlemeniz](label-entity-example-utterance.md) gerekmez. Tüm niyetlerle tüm sözlerle uygulanır.

## <a name="import-example-json-to-begin-app"></a>Uygulama başlatmak için alma örneği .json

1.  Uygulamayı indirin ve kaydedin [JSON dosyasını.](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="create-intent-for-sending-confirmation-text-messages"></a>Onay metin iletileri göndermek için niyet oluşturma

1. Bir sözcük teyidinin onay metni gönderme niyetini sınıflandırmak için yeni bir amaç oluşturmak için **+ Oluştur'u** seçin.

1. Açılan iletişim kutusuna `ConfirmationText` girip **Done** (Bitti) öğesini seçin.

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |x123432 pizza teslim süresi gönder|
    |Zaman için Txt x234567|
    |bildirim için x23987|

    Makinede öğrenilen varlıkları ayıklamak için, varlığı çeşitli söyleyişlerle içeren örnekler vermelisiniz, ancak bu makine de öğrenilmemiş varlıkla, varyasyon önemli değildir. Metin normal ifadeyle eşleşeceği sürece ayıklanır.

## <a name="use-the-regular-expression-entity-for-well-formatted-data"></a>İyi biçimlendirilmiş veriler için normal ifade varlığını kullanma
Metin numarasıyla eşleşecek normal bir ifade varlığı oluşturun. Bu normal ifade metinle eşleşir, ancak büyük/küçük harf ve kültür türevlerini yok sayar.

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

1. Varlıklar listesi sayfasında **+ Oluştur'u** seçin.

1. Açılan iletişim kutusunda, yeni varlık adını `ConfirmationTextRegEx`girin , varlık türü olarak **RegEx'i** seçin ve sonra **İleri'yi**seçin.

    > [!div class="mx-imgBorder"]
    > ![Normal ifade varlığı için varlık oluşturma adımlarını başlatın](./media/luis-quickstart-intents-regex-entity/pizza-create-new-entity.png)

1. **Regex varlık**oluştur'da, `x\d{5,6}` **Regex** değeri olarak girin ve sonra **Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Örnek söyleyişten veri ayıklamak için düzenli ifade girin](./media/luis-quickstart-intents-regex-entity/pizza-set-regular-expression-for-new-entity.png)

1. Sol menüden **Niyetler'i** seçin, ardından sözcüklerle etiketlenmiş normal ifadeyi görmek için **Onay Metni'ni** seçin.

    > [!div class="mx-imgBorder"]
    > ![Örnek ifadelerle etiketlenmiş normal ifadeyi görüntüleme](./media/luis-quickstart-intents-regex-entity/pizza-reg-ex-entity-shown-example-utterances-intent.png)

    Varlık makinede öğrenilen bir varlık olmadığından, varlık söyleyişe uygulanır ve oluşturulduğu anda LUIS portalında görüntülenir.

## <a name="train-the-app-before-testing-or-publishing"></a>Uygulamayı test etmeden veya yayınlamadan önce eğitin

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak için yayımlama

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Bitiş noktasından niyet ve varlık tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Adresteki URL'nin sonuna gidin ve aşağıdaki söyleyiyi girin:

    `Text my pizza delivery to x23456 x234567 x12345`

    Son sorgu dizesi parametresi ifade **s**orgusu olan `query` öğesidir.

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

* [Kavram - varlıklar](luis-concept-entity-types.md)
* [Normal ifade varlığının JSON referansı](reference-entity-regular-expression.md?tabs=V3)
* [Veri ayıklamak için varlıklar ekleme](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu, örnek konuşmalar eklendi ve ardından konuşmalardan düzgün biçimlendirilmiş veriler ayıklamak için normal ifade varlığı eklendi. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Liste varlığı hakkında bilgi edinin](tutorial-list-entity.md)

