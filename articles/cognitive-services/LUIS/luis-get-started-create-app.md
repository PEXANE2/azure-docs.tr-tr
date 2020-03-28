---
title: 'Quickstart: uygulama oluşturmak - LUIS'
description: Bu hızlı başlangıç, ışıkları ve cihazları açıp kapatmak `HomeAutomation` için önceden oluşturulmuş etki alanını kullanan bir LUIS uygulamasının nasıl oluşturulup oluşturulabildiğini gösterir. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
ms.topic: quickstart
ms.date: 03/24/2020
ms.openlocfilehash: de6cf5e95ee63fc9500cf1b5edab78597bdb18af
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80287807"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Hızlı başlangıç: Önceden oluşturulmuş ev otomasyonu uygulamasını kullanma

Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
**Uygulamalarım** sayfasından uygulama oluşturabilir ve yönetebilirsiniz.

1. Uygulamalar listemde, **sohbet için + Yeni uygulamayı**seçin.

1. İletişim kutusunda, uygulamanızı `Home Automation`adlandırın. Kültür olarak **İngilizce'yi** seçin. Açıklama isteğe bağlıdır ve yazma veya tahmin için kullanılmaz. Bir LUIS uygulaması oluştururken tahmin kaynağı da isteğe bağlıdır. **Done** (Bitti) öğesini seçin.

    Uygulamayı LUIS oluşturur. Uygulamanızı üretime yayımladığınızda, uygulamanızın birçok isteği işleyebilir diye bir tahmin kaynağı atamanız gerekir.

    ![İletişim kutusunda, uygulamanızı 'Ev Otomasyonu' olarak adlandırın](./media/create-new-app-details.png)

    >[!NOTE]
    >Uygulama oluşturduktan sonra kültür değiştirilemez.

## <a name="add-prebuilt-domain"></a>Önceden oluşturulmuş etki alanını ekleme

**Önceden oluşturulmuş etki alanlarını** seçin ve **HomeAutomation'ı**arayın. HomeAutomation kartında **etki alanı ekle'yi** seçin.

!['Önceden oluşturulmuş etki alanları'nı seçin ve 'HomeAutomation'ı arayın. HomeAutomation kartında 'Etki alanı ekle'yi seçin.](media/luis-quickstart-new-app/home-automation.png)

Etki alanı başarıyla eklendiğinde önceden oluşturulmuş etki alanı kutusunda **Etki alanını kaldır** düğmesi görüntülenir.

## <a name="intents-and-entities"></a>Amaçlar ve varlıklar

HomeAutomation etki alanı amaçlarını incelemek için **Niyetler'i** seçin. Önceden oluşturulmuş etki alanı niyetleri örnek söyleyinmeler var.

![HomeAutomation niyet listesinin ekran görüntüsü](media/luis-quickstart-new-app/home-automation-intents.png "HomeAutomation niyet listesinin ekran görüntüsü")

> [!NOTE]
> **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz.

**HomeAutomation.TurnOff** amacını seçin. Amaçta varlıklarla etiketlenmiş olan konuşmaların bir listesini görebilirsiniz.

[![HomeAutomation.TurnOff amacının ekran görüntüsü](media/luis-quickstart-new-app/home-automation-turnoff.png "HomeAutomation.TurnOff amacının ekran görüntüsü")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz.

1. Sağ üst gezintiden **Test'i** seçin. 1. Etkileşimli test bölmesine gibi `Turn off the lights` bir test sözcük yazın ve Enter tuşuna basın.

    ```
    Turn off the lights
    ```

    En yüksek puana sahip olan amacın test konuşması için beklediğiniz amaca karşılık geldiğinden emin olun.

    Bu örnekte, `Turn off the lights` **homeautomation.TurnOff'un**en yüksek puanlama amacı doğru olarak tanımlanır.

    ![Konuşmanın vurgulandığı Test paneli ekran görüntüsü](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Tahmin hakkında daha fazla bilgi gözden geçirmek için **İncele'yi** seçin.

    ![Denetim bilgileriile Test paneliekran görüntüsü](media/luis-quickstart-new-app/test.png)

1. Test bölmesini kapatın.

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)]

<a name="query-the-v2-api-prediction-endpoint"></a>

## <a name="query-the-v3-api-prediction-endpoint"></a>V3 API tahmin bitiş noktasını sorgula

[!INCLUDE [LUIS How to get endpoint first step](./includes/v3-prediction-endpoint.md)]

2. Tarayıcı adresi çubuğunda, sorgu dizesi için aşağıdaki ad ve değer çubuklarının URL'de olduğundan emin olun. Sorgu dizesinde değillerse, bunları ekleyin:

    |Ad/değer çifti|
    |--|
    |`verbose=true`|
    |`show-all-intents=true`|

3. Tarayıcı adresi çubuğunda, URL'nin sonuna gidin `turn off the living room light` ve _sorgu_ değeri için girin, ardından Enter tuşuna basın.

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

    [V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
