---
title: 'Öğretici: yayımlama ayarları-LUSıS'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, öngörülerden kazanmak için yayımlama ayarlarını değiştirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: diberry
ms.openlocfilehash: 7683a73fd1f403847089dc722e5f6382acd948cb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447688"
---
# <a name="tutorial--add-sentiment-analysis-as-a-publishing-setting"></a>Öğretici: yayımlama ayarı olarak yaklaşım Analizi ekleme

Bu öğreticide, yaklaşım analizini çıkarmak için yayımlama ayarlarını değiştirin ve ardından bir Kullanıcı utüterinin döndürdüğü yaklaşımı görmek için LUO uç noktasını sorgulayın.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Yayımlama ayarı olarak yaklaşım Analizi ekleme
> * Yayınlanan uç noktadan bir duygu duygunluklama edin

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Yaklaşım Analizi bir yayımlama ayarıdır

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Duygu|Puan|İfade|
|:--|:--|:--|
|negatif|0.01 |Pizza başlandı.|
|pozitif|0,97 |Cheese pizza harika.|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir yayımlama ayarıdır. Bir kez ayarlandıktan sonra uygulamanız, verileri etiketlemenize gerek kalmadan söylenişi 'in yaklaşımını döndürür.

Bir yayımlama ayarı olduğundan, bu dosyayı amaçlar veya varlıklar sayfalarında Not olarak görmezsiniz. [Etkileşimli test](luis-interactive-test.md#view-sentiment-results) bölmesinde veya uç nokta URL'sinde test yaparken görebilirsiniz.

## <a name="import-example-json-to-begin-app"></a>Uygulamayı başlatmak için example. JSON al

1.  [Uygulama json dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)indirip kaydedin.

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma

1. Üstteki menüden **Yayımla** ' yı seçin. Yaklaşım Analizi bir yayımlama ayarıdır.

1. **Üretim yuvasını** seçin ve **Ayarları Değiştir**' i seçin.
1. Yaklaşım Analizi ayarını **Açık**olarak ayarlayın.

    ![Yayımlama ayarı olarak üzerinde yaklaşım analizi Aç](./media/luis-quickstart-intent-and-sentiment-analysis/select-sentiment-publishing-setting.png)

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Uç noktadan bir duygu duygunluyararlanın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adresteki URL 'nin sonuna gidin ve aşağıdaki deterance 'i girin:

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

    Yaklaşım analizi, %86 puanı ile pozitif bir değer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* Yaklaşım analizi, bilişsel hizmet [metin analizi](../Text-Analytics/index.yml)tarafından sağlanır. Özellik [desteklenen diller](luis-language-support.md##languages-supported)metin analizi kısıtlıdır.
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, bir bütün olarak konuşmadan yaklaşım değerleri ayıklamak için bir yayımlama ayarı olarak yaklaşım analizi ekler.

> [!div class="nextstepaction"]
> [İK uygulamasında uç nokta konuşmasını gözden geçirme](luis-tutorial-review-endpoint-utterances.md)

