---
title: 'Öğretici: yayımlama ayarları-LUSıS'
description: Bu öğreticide, öngörülerden kazanmak için yayımlama ayarlarını değiştirin.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: 19913d16ecb1457ad4edb93ea34e4b96a590aca0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80545766"
---
# <a name="tutorial--add-sentiment-analysis-as-a-publishing-setting"></a>Öğretici: yayımlama ayarı olarak yaklaşım Analizi ekleme

Bu öğreticide, yaklaşım analizini çıkarmak için yayımlama ayarlarını değiştirin ve ardından bir Kullanıcı utüterinin döndürdüğü yaklaşımı görmek için LUO uç noktasını sorgulayın.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Yayımlama ayarı olarak yaklaşım Analizi ekleme
> * Yayınlanan uç noktadan bir duygu duygunluklama edin

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Yaklaşım Analizi bir yayımlama ayarıdır

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım|Puan|İfade|
|:--|:--|:--|
|negatif|0,01 |Pizza başlandı.|
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

    ![Yayımlama ayarı olarak Yaklaşım Analizi aç](./media/luis-quickstart-intent-and-sentiment-analysis/select-sentiment-publishing-setting.png)

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Uç noktadan bir duygu duygunluyararlanın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ ile değiştirin:

    `Deliver 2 of the best cheese pizzas ever!!!`

    Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `OrderPizza` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.

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

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* Yaklaşım analizi, bilişsel hizmet [metin analizi](../Text-Analytics/index.yml)tarafından sağlanır. Özellik [desteklenen diller](luis-language-support.md#languages-supported)metin analizi kısıtlıdır.
* [Eğitme](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, bir bütün olarak konuşmadan yaklaşım değerleri ayıklamak için bir yayımlama ayarı olarak yaklaşım analizi ekler.

> [!div class="nextstepaction"]
> [İK uygulamasında uç nokta konuşmasını gözden geçirme](luis-tutorial-review-endpoint-utterances.md)

