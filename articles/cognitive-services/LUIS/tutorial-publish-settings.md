---
title: 'Öğretici: Ayarları yayımla - LUIS'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, tahminleri geliştirmek için yayımlama ayarlarını değiştirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: diberry
ms.openlocfilehash: 2df32c20bebf4243f383a0cccd8f037721533602
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75890383"
---
# <a name="tutorial--add-sentiment-analysis-as-a-publishing-setting"></a>Öğretici: Yayımlama ayarı olarak duygu analizi ekleme

Bu öğreticide, duyarlılık analizini ayıklamak için yayımlama ayarlarını değiştirin ve ardından kullanıcı nın söyleyerek döndürülen duygularını görmek için LUIS bitiş noktasını sorgulayın.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Yayımlama ayarı olarak duyarlılık analizi ekleme
> * Yayınlanan bitiş noktasından bir söyleyiş duygusu alın

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Duygu analizi bir yayın ayarıdır

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım|Puan|İfade|
|:--|:--|:--|
|negatif|0.01 |Pizza berbattı.|
|pozitif|0.97 |Peynirli pizza harikaydı.|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir yayımlama ayarıdır. Ayarlandıktan sonra, uygulamanız verileri etiketlemenize gerek kalmadan bir söyleyiş hissini geri verir.

Yayımlama ayarı olduğundan, niyet veya varlıklar sayfalarında etiketlendiğini görmezsiniz. [Etkileşimli test](luis-interactive-test.md#view-sentiment-results) bölmesinde veya uç nokta URL'sinde test yaparken görebilirsiniz.

## <a name="import-example-json-to-begin-app"></a>Uygulama başlatmak için alma örneği .json

1.  Uygulamayı indirin ve kaydedin [JSON dosyasını.](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma

1. Üst menüden **Yayımla'yı** seçin. Duygusallık analizi bir yayın ayarıdır.

1. **Üretim yuvasını** seçin ve **ardından Ayarları Değiştir'i**seçin.
1. Duygu Analizi ayarını **Ayarı'na**ayarlayın.

    ![Yayımlama ayarı olarak Sentiment Analysis'i açın](./media/luis-quickstart-intent-and-sentiment-analysis/select-sentiment-publishing-setting.png)

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Son noktadan bir söyleyiş duygusu alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adresteki URL'nin sonuna gidin ve aşağıdaki söyleyiyi girin:

    `Deliver 2 of the best cheese pizzas ever!!!`

    Son sorgu dizesi parametresi ifade **s**orgusu olan `query` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `OrderPizza` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.

    ```json
    {
        "query": "Deliver 2 of the best cheese pizzas ever!!!",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.4659952
                },
                "None": {
                    "score": 0.16901511
                },
                "Confirm": {
                    "score": 0.00421415
                },
                "Greeting": {
                    "score": 0.00109914376
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "$instance": {
                    "number": [
                        {
                            "type": "builtin.number",
                            "text": "2",
                            "startIndex": 8,
                            "length": 1,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            },
            "sentiment": {
                "label": "positive",
                "score": 0.861665964
            }
        }
    }
    ```

    Duyarlılık analizi %86'lık bir puanla olumlu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* Duyarlılık analizi Bilişsel Hizmet [Metin Analizi](../Text-Analytics/index.yml)tarafından sağlanmaktadır. Bu özellik Metin Analizi [tarafından desteklenen diller](luis-language-support.md#languages-supported)ile sınırlıdır.
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, bir bütün olarak konuşmadan yaklaşım değerleri ayıklamak için bir yayımlama ayarı olarak yaklaşım analizi ekler.

> [!div class="nextstepaction"]
> [İK uygulamasında uç nokta konuşmasını gözden geçirme](luis-tutorial-review-endpoint-utterances.md)

