---
title: Python'da REST araması ile niyet edin
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/20/2020
ms.author: diberry
ms.openlocfilehash: 2307ef6ea8940a3b3ddfb8c7539f4f809dc4c52c
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733253"
---
## <a name="prerequisites"></a>Ön koşullar

* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* [Visual Studio Code](https://code.visualstudio.com/)
* Bir LUIS uygulama kimliği - genel IoT uygulama kimliğini `df67dcdb-c37d-46af-88e1-8b97951ca1c2`kullanın. Quickstart kodunda kullanılan kullanıcı sorgusu bu uygulamaya özgüdür.

## <a name="create-luis-runtime-key-for-predictions"></a>Öngörüler için LUIS çalışma zamanı anahtarı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açma
1. [Dil **Anlama Oluştur'u** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) tıklatın
1. **Runtime** tuşu için gerekli tüm ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenilen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`- minimum fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçme|

1. **Oluştur'u** tıklatın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin.
1. Yapılandırılmış `endpoint` ve bir `key`toplamak .

## <a name="get-intent-from-the-prediction-endpoint"></a>Tahmin bitiş noktasından niyet alma

[Tahmin bitiş noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucunu almak için Python'u kullanın.

1. Bu kod parçacığının bir dosyaya `predict.py`kopyalanması:

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

1. `YOUR-KEY` Ve `YOUR-ENDPOINT` değerleri kendi tahmininizle **Runtime** tuşu ve bitiş noktasıile değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter tahmini **Runtime** anahtarınız.|
    |`YOUR-ENDPOINT`| Tahmin URL bitiş noktanız. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Bağımlılığı `requests` yükleyin. Bu, HTTP isteklerini yapmak için kullanılır:

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

    Burada okunabilirlik için biçimlendirilmiş JSON yanıtı:

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

Bu hızlı başlatmayı bitirdiğinizde, dosyayı dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Söyleyiş ve tren ekle](../get-started-get-model-rest-apis.md)