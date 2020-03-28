---
title: 'Quickstart: Tarayıcı ile niyet alın - LUIS'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir kullanıcının tarayıcıdaki konuşma metninden niyetini belirlemek için kullanılabilir bir genel LUIS uygulamasını kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: diberry
ms.openlocfilehash: e06bb4c09b3ebab25c0c0ef8ac5c51f6842f34cd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987963"
---
# <a name="quickstart-get-intent-with-a-browser"></a>Quickstart: Bir tarayıcı ile niyet alın

Bir LUIS tahmin uç noktasının ne döndüğünü anlamak için bir tahmin sonucunu bir Web tarayıcısında görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

Genel bir uygulamayı sorgulamak için şunları yapmanız gerekir:

* [LUIS Portal (Önizleme)](https://preview.luis.ai/)elde edilebilir kendi Dil Anlayışı (LUIS) Yazma veya Tahmin anahtarı. Bir anahtar oluşturmak için zaten bir aboneliğiniz yoksa, [ücretsiz](https://azure.microsoft.com/free/)bir hesaba kaydolabilirsiniz.
* Genel uygulamanın kimliği: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`.

## <a name="use-the-browser-to-see-predictions"></a>Tahminleri görmek için tarayıcıyı kullanın

1. Bir web tarayıcısı açın.
1. Kendi LUIS Yazma veya Tahmin `YOUR-KEY` anahtarınızı değiştirerek aşağıdaki tüm URL'leri kullanın. İstekler GET istekleridir ve luis Yazma veya Tahmin anahtarınızla birlikte bir sorgu dizesi parametresi olarak yetkilendirmeyi içerir.

    #### <a name="v3-prediction-request"></a>[V3 tahmin isteği](#tab/V3-1-1)


    **GET** bitiş noktası (yuvalara göre) isteği için V3 URL'sinin biçimi:

    `
    https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY
    `

    #### <a name="v2-prediction-request"></a>[V2 tahmin isteği](#tab/V2-1-2)

    **GET** bitiş noktası isteği için V2 URL'sinin biçimi:

    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-KEY&q=turn on all lights
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
    https://westus.api.cognitive.microsoft.com/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY&show-all-intents=true
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
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key={your-key}&verbose=true
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


<!-- FIX - is the public app getting updated for the new prebuilt domain with entities? -->

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [LUIS portalında uygulama oluşturma](get-started-portal-build-app.md)
