---
title: 'Hızlı başlangıç: uygulama oluşturma-LUSıS'
titleSuffix: Azure Cognitive Services
description: Işıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturun. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/05/2019
ms.author: diberry
ms.openlocfilehash: 1704b62cae6375d376fc43fb7a2940cd9c717072
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382517"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Hızlı Başlangıç: Önceden oluşturulmuş giriş Otomasyonu uygulamasını kullanma

Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu makale için [https://www.luis.ai](https://www.luis.ai) adresindeki LUIS portalından oluşturulan ücretsiz bir LUIS hesabına ihtiyacınız olacak. 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
**Uygulamalarım** sayfasından uygulama oluşturabilir ve yönetebilirsiniz. 

2. **Yeni uygulama oluştur**'u seçin.

    [![Uygulama listesinin ekran görüntüsü](media/luis-quickstart-new-app/app-list.png "uygulama listesinin ekran görüntüsü")](media/luis-quickstart-new-app/app-list.png)

3. İletişim kutusunda uygulamanıza "Home Automation" adını verin.

    [![Yeni uygulama açılır iletişim Oluştur ekran görüntüsü](media/luis-quickstart-new-app/create-new-app-dialog.png "yeni uygulama açılır iletişim Oluştur ekran görüntüsü")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Uygulamanızın kültürünü seçin. Bu Home Automation uygulaması için İngilizce seçeneğini belirleyin. Ardından **Bitti**'yi seçin. LUIS, Home Automation uygulamasını oluşturur. 

    >[!NOTE]
    >Uygulama oluşturduktan sonra kültür değiştirilemez. 

## <a name="add-prebuilt-domain"></a>Önceden oluşturulmuş etki alanını ekleme

Sol taraftaki gezinti bölmesinden **Önceden oluşturulmuş etki alanları**'nı seçin. Ardından "Home" araması yapın. **Etki alanı ekle**'yi seçin.

[![Ekran görüntüsü, giriş Otomasyon etki alanı çekilerek önceden oluşturulmuş etki alanı menüde](media/luis-quickstart-new-app/home-automation.png "ekran görüntüsü, giriş Otomasyon etki alanı çekilerek önceden oluşturulmuş etki alanı menüsü")](media/luis-quickstart-new-app/home-automation.png)

Etki alanı başarıyla eklendiğinde önceden oluşturulmuş etki alanı kutusunda **Etki alanını kaldır** düğmesi görüntülenir.

[![Kaldır düğmesinin Ekran görüntüsü, giriş Otomasyon etki](media/luis-quickstart-new-app/remove-domain.png "Kaldır düğmesinin Ekran görüntüsü, giriş Otomasyon etki alanı")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Amaçlar ve varlıklar

HomeAutomation etki alanının amaçlarını incelemek için sol taraftaki gezinti bölmesinden **Amaçlar**'ı seçin. Her amaçta örnek konuşmalar bulunur.

![HomeAutomation amaçları listesinin ekran görüntüsü](media/luis-quickstart-new-app/home-automation-intents.png "HomeAutomation hedefleri listesinin ekran görüntüsü")]

> [!NOTE]
> **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz. 

**HomeAutomation.TurnOff** amacını seçin. Amaçta varlıklarla etiketlenmiş olan konuşmaların bir listesini görebilirsiniz.

[![Ekran görüntüsü, HomeAutomation.TurnOff hedefi](media/luis-quickstart-new-app/home-automation-turnoff.png "ekran görüntüsü, HomeAutomation.TurnOff hedefi")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz. Üst gezinti bölmesinde **Test**'i seçin. Etkileşimli Test bölmesine "Turn off the lights" (Işıkları kapat) gibi bir test konuşması yazın. 

```
Turn off the lights
```

En yüksek puana sahip olan amacın test konuşması için beklediğiniz amaca karşılık geldiğinden emin olun.

Bu örnekte, `Turn off the lights` **homeautomation. turnoff**'un en üst Puanlama hedefi olarak doğru şekilde tanımlanır.

[![Utterance vurgulandığı ekran görüntüsü, Test paneliyle](media/luis-quickstart-new-app/test.png "utterance vurgulandığı ekran görüntüsü, Test paneli")](media/luis-quickstart-new-app/test.png)


Tahmin hakkında daha fazla **bilgi için İnceleme** ' yi seçin.

![Söylenişi vurgulanmış şekilde test paneli ekran görüntüsü](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

Test bölmesini daraltmak için yeniden **Test**'i seçin. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-v2-api-prediction-endpoint"></a>V2 API tahmini uç noktasını sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Adresteki URL'nin sonuna gidin ve `turn off the living room light` tümcesini girip Enter tuşuna basın. Tarayıcı, HTTP uç noktanızın JSON yanıtının **v2 API** sürümünü görüntüler.

    ```json
    {
      "query": "turn off the living room light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.9753089
      },
      "intents": [
        {
          "intent": "HomeAutomation.TurnOff",
          "score": 0.9753089
        },
        {
          "intent": "HomeAutomation.QueryState",
          "score": 0.01027893
        },
        {
          "intent": "HomeAutomation.TurnUp",
          "score": 0.006881481
        },
        {
          "intent": "HomeAutomation.SetDevice",
          "score": 0.006786365
        },
        {
          "intent": "HomeAutomation.TurnDown",
          "score": 0.005145787
        },
        {
          "intent": "HomeAutomation.TurnOn",
          "score": 0.004114749
        },
        {
          "intent": "None",
          "score": 0.000598924
        }
      ],
      "entities": [
        {
          "entity": "living room",
          "type": "HomeAutomation.Location",
          "startIndex": 13,
          "endIndex": 23,
          "score": 0.94558233
        },
        {
          "entity": "living room light",
          "type": "HomeAutomation.DeviceName",
          "startIndex": 13,
          "endIndex": 29,
          "resolution": {
            "values": [
              "living room light"
            ]
          }
        },
        {
          "entity": "light",
          "type": "HomeAutomation.DeviceType",
          "startIndex": 25,
          "endIndex": 29,
          "resolution": {
            "values": [
              "light"
            ]
          }
        }
      ]
    }
    ```
    
## <a name="query-the-v3-api-prediction-endpoint"></a>V3 API tahmini uç noktasını sorgulama

Bir [v3 API sorgusu](luis-migration-api-v3.md)için, tarayıcıda alma yöntemi https isteğini değiştirin, değerleri kendi değerlerinizin açılı ayraçlar cinsinden değiştirin. 

**Get yöntemi Ile v2 URL 'si**:

https://\<Region >. api. bilişsel. Microsoft. com/lusıs/**v 2.0**\</Apps/AppID >? verbose = true & Subscription-Key\<= YOUR_KEY > &**q\<= Kullanıcı-utterance-Text >**

**Get yöntemi Ile v3 URL 'si**:

https://\<Region >. api.**bilişsel**. Microsoft. com/lusıs **/v 3.0-Preview**/Apps/\<AppID >/**yuvalar**/**üretimi**/tahmin mi? verbose = true & abonelik-anahtar =\<YOUR_KEY > &**sorgu =\<Kullanıcı-utterance-metin >**

Tarayıcı, HTTP uç noktanızın JSON yanıtının **v3 API** sürümünü görüntüler.

```json
{
    "query": "turn off the lights",
    "prediction": {
        "normalizedQuery": "turn off the lights",
        "topIntent": "HomeAutomation.TurnOff",
        "intents": {
            "HomeAutomation.TurnOff": {
                "score": 0.99649024
            }
        },
        "entities": {
            "HomeAutomation.DeviceType": [
                [
                    "light"
                ]
            ],
            "$instance": {
                "HomeAutomation.DeviceType": [
                    {
                        "type": "HomeAutomation.DeviceType",
                        "text": "lights",
                        "startIndex": 13,
                        "length": 6,
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

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
