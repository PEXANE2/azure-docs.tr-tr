---
title: 'Hızlı başlangıç: tarayıcı için amaç al-LUSıS'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanıcının bir tarayıcıda konuşma metinleriyle aynı olduğunu öğrenmek için kullanılabilir bir genel LUL uygulaması kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 6282e768ebc51d0d4ec2b15f057727f207a7d81a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703136"
---
# <a name="quickstart-get-intent-with-a-browser"></a>Hızlı başlangıç: bir tarayıcıyla amaç alın

Bir LUIS tahmin uç noktasının ne döndüğünü anlamak için bir tahmin sonucunu bir Web tarayıcısında görüntüleyin. 

## <a name="prerequisites"></a>Önkoşullar

Bir ortak uygulamayı sorgulamak için şunlar gerekir:

* Kendi Language Understanding (LUSıS) anahtarınız. Zaten anahtar oluşturmak için bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)için kaydolabilirsiniz.
* Ortak uygulamanın KIMLIĞI: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. 

## <a name="use-the-browser-to-see-predictions"></a>Tahminleri görmek için tarayıcıyı kullanın

1. Bir web tarayıcısı açın. 
1. Aşağıdaki tam URL 'Leri kullanın ve `{your-key}` ' ı kendi LUSıS anahtarınızla değiştirin. İstekler GET isteklerdir ve bir sorgu dizesi parametresi olarak LUSıS anahtarınızla yetkilendirmeyi içerir.

    #### <a name="v2-prediction-endpoint-requesttabv2"></a>[V2 tahmin uç noktası isteği](#tab/V2)
    
    Bir **Get** Endpoint Isteği IÇIN v2 URL 'sinin biçimi:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key={your-key}&q=turn on all lights
    `
    
    #### <a name="v3-prediction-endpoint-requesttabv3"></a>[V3 tahmin uç noktası isteği](#tab/V3)
    
    
    Bir **Get** Endpoint (yuvalara göre) Isteği IÇIN v3 URL 'sinin biçimi:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}
    `
    
    * * *

1. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. Tarayıcıda LUIS'in `HomeAutomation.TurnOn` amacını en önemli amaç olarak ve `HomeAutomation.Operation` varlığını `on` değeriyle algıladığını gösteren bir JSON sonucu görüntülenir.

    #### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

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

    #### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
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

    * * *

1. Tüm hedefleri görmek için uygun sorgu dizesi parametresini ekleyin. 

    #### <a name="v2-prediction-endpointtabv2"></a>[V2 tahmin uç noktası](#tab/V2)

    **Tüm amaçları göstermek**için QueryString 'in sonuna `verbose=true` ekleyin:

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

    #### <a name="v3-prediction-endpointtabv3"></a>[V3 tahmin uç noktası](#tab/V3)

    **Tüm amaçları göstermek**için QueryString 'in sonuna `show-all-intents=true` ekleyin:

    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "HomeAutomation.TurnOff": {
                     "score": 0.0207554
                },
                "None": {
                     "score": 0.08687421
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

    * * * 

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [LUSıS portalında uygulama oluşturma](get-started-portal-build-app.md)