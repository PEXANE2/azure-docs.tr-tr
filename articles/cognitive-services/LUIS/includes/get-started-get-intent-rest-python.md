---
title: Python 'da REST çağrısıyla amacı al
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/20/2020
ms.author: diberry
ms.openlocfilehash: 2307ef6ea8940a3b3ddfb8c7539f4f809dc4c52c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733253"
---
## <a name="prerequisites"></a>Ön koşullar

* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* [Visual Studio Code](https://code.visualstudio.com/)
* Bir LUSıS uygulama KIMLIĞI-genel IoT uygulama KIMLIĞI ' ni kullanın `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Hızlı başlangıç kodunda kullanılan Kullanıcı sorgusu bu uygulamaya özeldir.

## <a name="create-luis-runtime-key-for-predictions"></a>Tahminler için LUSıS çalışma zamanı anahtarı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [Oluştur **Language Understanding** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) tıklayın
1. **Çalışma zamanı** anahtarı için tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`-Minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin.
1. Toplama yapılandırılmış `endpoint` ve bir `key`.

## <a name="get-intent-from-the-prediction-endpoint"></a>Tahmin uç noktasından amaç al

[Tahmin uç noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucu almak için Python kullanın.

1. Bu kod parçacığını adlı `predict.py`bir dosyaya kopyalayın:

    ```python
    ########### Python 3.6 #############
    import requests

    try:

        key = 'YOUR-KEY' # your Runtime key
        endpoint = 'YOUR-ENDPOINT' # such as 'your-resource-name.api.cognitive.microsoft.com'
        appId = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
        utterance = 'turn on all lights'

        headers = {
        }

        params ={
            'query': utterance,
            'timezoneOffset': '0',
            'verbose': 'true',
            'show-all-intents': 'true',
            'spellCheck': 'false',
            'staging': 'false',
            'subscription-key': key
        }

        r = requests.get(f'https://{endpoint}/luis/prediction/v3.0/apps/{appId}/slots/production/predict',headers=headers, params=params)
        print(r.json())

    except Exception as e:
        print(f'{e}')
    ```

1. `YOUR-KEY` Ve `YOUR-ENDPOINT` değerlerini kendi tahmin **çalışma zamanı** anahtarınızla ve uç noktanızla değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter tahmini **çalışma zamanı** anahtarınız.|
    |`YOUR-ENDPOINT`| Tahmin URL 'niz uç noktasıdır. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. `requests` Bağımlılığı yükler. Bu, HTTP isteklerini yapmak için kullanılır:

    ```console
    pip install requests
    ```

1. Komut dosyanızı bu konsol komutuyla çalıştırın:

    ```console
    python predict.py
    ```

1. JSON olarak döndürülen tahmin yanıtını gözden geçirin:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    Okunabilirlik için biçimlendirilen JSON yanıtı aşağıda verilmiştir:

    ```JSON
    {
        "query": "turn on all lights",
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
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
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

Bu hızlı başlangıcı tamamladığınızda dosyayı dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Utterslar ve tren ekleme](../get-started-get-model-rest-apis.md)