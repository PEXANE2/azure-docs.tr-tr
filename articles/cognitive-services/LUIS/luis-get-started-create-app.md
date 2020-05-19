---
title: 'Hızlı başlangıç: uygulama oluşturma-LUSıS'
description: Bu hızlı `HomeAutomation` Başlangıçta, ışıkları ve gereçleri açmak ve kapatmak için önceden oluşturulmuş etki alanını kullanan BIR Luo uygulamasının nasıl oluşturulacağı gösterilmektedir. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
ms.topic: quickstart
ms.date: 05/05/2020
ms.openlocfilehash: 186a60a7fd8315d68718ceedd3b5cadb4d3645e8
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589185"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Hızlı başlangıç: Önceden oluşturulmuş ev otomasyonu uygulamasını kullanma

Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
**Uygulamalarım** sayfasından uygulama oluşturabilir ve yönetebilirsiniz.

1. Uygulamalarım listesinde, **konuşma için + yeni uygulama**' yı seçin ve ardından Seçenekler listesinde, yeniden **konuşmaya yönelik yeni uygulama** ' yı seçin.

1. İletişim kutusunda uygulamanızı adlandırın `Home Automation` .
1. Kültür olarak **İngilizce** ' yi seçin.
1. İsteğe bağlı bir açıklama girin.
1. Kaynağı henüz oluşturmadıysanız bir tahmin kaynağı seçmeyin. Uygulamanızın tahmin uç noktasını (hazırlama veya üretim) kullanmak için bir tahmin kaynağı atamanız gerekir.
1. **Done** (Bitti) öğesini seçin.

    LUO uygulamayı oluşturur.

    ![İletişim kutusunda, uygulamanızın giriş Otomasyonu ' nu adlandırın](./media/create-new-app-details.png)

    >[!NOTE]
    >Uygulama oluşturduktan sonra kültür değiştirilemez.

## <a name="add-prebuilt-domain"></a>Önceden oluşturulmuş etki alanını ekleme

1. Sol gezinti bölmesinde, **önceden oluşturulmuş etki alanları**' nı seçin.
1. **Homeautomation**için arama yapın.
1. HomeAutomation kartında **etki alanı Ekle** ' yi seçin.

    ![' Önceden oluşturulmuş etki alanları ' ' nı seçin ve ' HomeAutomation ' araması yapın. HomeAutomation kartında ' etki alanı Ekle ' seçeneğini belirleyin.](media/luis-quickstart-new-app/home-automation.png)

    Etki alanı başarıyla eklendiğinde önceden oluşturulmuş etki alanı kutusunda **Etki alanını kaldır** düğmesi görüntülenir.

## <a name="intents-and-entities"></a>Amaçlar ve varlıklar

1. HomeAutomation etki alanı amaçlarını gözden geçirmek için **amaçlar** ' ı seçin. Önceden oluşturulmuş etki alanı amaçları, örnek söylenebilir.

    ![HomeAutomation amaçları listesinin ekran görüntüsü](media/luis-quickstart-new-app/home-automation-intents.png "HomeAutomation amaçları listesinin ekran görüntüsü")

    > [!NOTE]
    > **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz.

1. **HomeAutomation.TurnOff** amacını seçin. Amaç, varlıklarla etiketlenmiş örnek bir listesini içerir.

    > [!div class="mx-imgBorder"]
    > [![HomeAutomation.TurnOff amacının ekran görüntüsü](media/luis-quickstart-new-app/home-automation-turnoff.png "HomeAutomation.TurnOff amacının ekran görüntüsü")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz.

1. Sağ üst gezinmede **Test** ' i seçin.

1. Etkileşimli test bölmesine benzer bir test söyliği yazın `Turn off the lights` ve ENTER tuşuna basın.

    ```
    Turn off the lights
    ```

    Bu örnekte, `Turn off the lights` **Homeautomation. turnoff**'un en üst Puanlama hedefi olarak doğru şekilde tanımlanır.

    ![Konuşmanın vurgulandığı Test paneli ekran görüntüsü](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Tahmin hakkında daha fazla bilgi görüntülemek için **İnceleme** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![İnceleme bilgileriyle test bölmesinin ekran görüntüsü](media/luis-quickstart-new-app/test.png)

1. Test bölmesini kapatın.

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)]

<a name="query-the-v2-api-prediction-endpoint"></a>

## <a name="query-the-v3-api-prediction-endpoint"></a>V3 API tahmini uç noktasını sorgulama

[!INCLUDE [LUIS How to get endpoint first step](./includes/v3-prediction-endpoint.md)]

2. Tarayıcı adres çubuğunda, sorgu dizesi için aşağıdaki ad ve değer çubuklarının URL 'de olduğundan emin olun. Sorgu dizesinde yoksa, bunları ekleyin:

    |Ad/değer çifti|
    |--|
    |`verbose=true`|
    |`show-all-intents=true`|

3. Tarayıcının adres çubuğunda URL 'nin sonuna gidin ve `turn off the living room light` _sorgu_ değeri Için yazın ve ENTER tuşuna basın.

    ```json
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.984315455
                },
                "HomeAutomation.QueryState": {
                    "score": 0.009912962
                },
                "HomeAutomation.TurnDown": {
                    "score": 0.00626645749
                },
                "HomeAutomation.TurnUp": {
                    "score": 0.00572059769
                },
                "HomeAutomation.SetDevice": {
                    "score": 0.00379381469
                },
                "HomeAutomation.TurnOn": {
                    "score": 0.00366983772
                },
                "None": {
                    "score": 0.000623856
                }
            },
            "entities": {
                "HomeAutomation.Location": [
                    "living room"
                ],
                "HomeAutomation.DeviceName": [
                    [
                        "living room light"
                    ]
                ],
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ],
                "$instance": {
                    "HomeAutomation.Location": [
                        {
                            "type": "HomeAutomation.Location",
                            "text": "living room",
                            "startIndex": 13,
                            "length": 11,
                            "score": 0.907323956,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceName": [
                        {
                            "type": "HomeAutomation.DeviceName",
                            "text": "living room light",
                            "startIndex": 13,
                            "length": 17,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceType": [
                        {
                            "type": "HomeAutomation.DeviceType",
                            "text": "light",
                            "startIndex": 25,
                            "length": 5,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
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

    [V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
