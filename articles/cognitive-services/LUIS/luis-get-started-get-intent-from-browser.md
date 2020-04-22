---
title: 'Quickstart: Tarayıcı ile tahmin sorgusu - LUIS'
description: Bu hızlı başlangıçta, bir kullanıcının tarayıcıdaki konuşma metninden niyetini belirlemek için kullanılabilir bir genel LUIS uygulamasını kullanın.
ms.topic: quickstart
ms.date: 04/21/2020
ms.openlocfilehash: 5ba86882ebf3cb538ad6b865382342fcbd43d27c
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769979"
---
# <a name="quickstart-query-prediction-runtime-with-user-text"></a>Quickstart: Kullanıcı metniyle tahmin çalışma süresini sorgula

Bir LUIS tahmin uç noktasının ne döndüğünü anlamak için bir tahmin sonucunu bir Web tarayıcısında görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

Genel bir uygulamayı sorgulamak için şunları yapmanız gerekir:

* Dil Anlayışınız (LUIS) kaynak bilgileriniz:
    * **Tahmin anahtarı** - LUIS [Portal](https://www.luis.ai/)elde edilebilir . Bir anahtar oluşturmak için zaten bir aboneliğiniz yoksa, [ücretsiz](https://azure.microsoft.com/free/)bir hesaba kaydolabilirsiniz.
    * **Tahmin bitiş noktası alt etki alanı** - alt etki alanı da LUIS kaynağınızın **adıdır.**
* Bir LUIS uygulama kimliği - genel IoT uygulama kimliğini `df67dcdb-c37d-46af-88e1-8b97951ca1c2`kullanın. Quickstart kodunda kullanılan kullanıcı sorgusu bu uygulamaya özgüdür.

## <a name="use-the-browser-to-see-predictions"></a>Tahminleri görmek için tarayıcıyı kullanın

1. Bir web tarayıcısı açın.
1. Kendi LUIS Prediction anahtarınızı değiştirerek `YOUR-KEY` aşağıdaki tüm URL'leri kullanın. İstekler GET istekleridir ve bir sorgu dizesi parametresi olarak LUIS Prediction anahtarınızla yetkilendirmeyi içerir.

    #### <a name="v3-prediction-request"></a>[V3 tahmin isteği](#tab/V3-1-1)


    **GET** bitiş noktası (yuvalara göre) isteği için V3 URL'sinin biçimi:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY
    `

    #### <a name="v2-prediction-request"></a>[V2 tahmin isteği](#tab/V2-1-2)

    **GET** bitiş noktası isteği için V2 URL'sinin biçimi:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-LUIS-PREDICTION-KEY&q=turn on all lights
    `

1. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. Tarayıcıda LUIS'in `HomeAutomation.TurnOn` amacını en önemli amaç olarak ve `HomeAutomation.Operation` varlığını `on` değeriyle algıladığını gösteren bir JSON sonucu görüntülenir.

    #### <a name="v3-prediction-response"></a>[V3 tahmin yanıtı](#tab/V3-2-1)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-response"></a>[V2 tahmin yanıtı](#tab/V2-2-2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    * * *

1. Tüm amaçları görmek için uygun sorgu dize parametresini ekleyin.

    #### <a name="v3-prediction-endpoint"></a>[V3 tahmin bitiş noktası](#tab/V3-3-1)

    Tüm `show-all-intents=true` **niyetlerini göstermek**için sorgu dizesinin sonuna ekleyin:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-endpoint"></a>[V2 tahmin bitiş noktası](#tab/V2)

    Tüm `verbose=true` **niyetlerini göstermek**için sorgu dizesinin sonuna ekleyin:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [V3 tahmin bitiş noktası](luis-migration-api-v3.md)
* [Özel alt etki alanları](../cognitive-services-custom-subdomains.md)

> [!div class="nextstepaction"]
> [LUIS portalında uygulama oluşturma](get-started-portal-build-app.md)
