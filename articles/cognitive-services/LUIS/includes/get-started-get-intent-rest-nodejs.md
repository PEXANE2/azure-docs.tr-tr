---
title: Node. js ' de REST çağrısıyla amacı al
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: c7354ecce9873cd65580dc2a9d79f9f3b8ac37db
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76966636"
---
## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/) programlama dili
* [Visual Studio Code](https://code.visualstudio.com/)
* Ortak uygulama KIMLIĞI: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`

## <a name="create-luis-runtime-key-for-predictions"></a>Tahminler için LUSıS çalışma zamanı anahtarı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [Oluştur **Language Understanding** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) tıklayın
1. Çalışma zamanı anahtarı için tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`-minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin.
1. Yapılandırılmış `endpoint` ve bir `key`toplayın.

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

[Tahmin uç noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucu almak için Node. js ' i kullanın.

1. Aşağıdaki kod parçacığını `predict.js`adlı bir dosyaya kopyalayın:

    ```javascript
    var request = require('request');
    var requestpromise = require('request-promise');
    var querystring = require('querystring');

    // Analyze text
    //
    getPrediction = async () => {

        // YOUR-KEY - Language Understanding runtime key
        var endpointKey = "YOUR-KEY";

        // YOUR-ENDPOINT Language Understanding endpoint URL, an example is your-resource-name.api.cognitive.microsoft.com
        var endpoint = "YOUR-ENDPOINT";

        // Set the LUIS_APP_ID environment variable
        // to df67dcdb-c37d-46af-88e1-8b97951ca1c2, which is the ID
        // of a public sample application.
        var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

        var utterance = "turn on all lights";

        // Create query string
        var queryParams = {
            "show-all-intents": true,
            "verbose":  true,
            "query": utterance,
            "subscription-key": endpointKey
        }

        // append query string to endpoint URL
        var URI = `https://${endpoint}/luis/prediction/v3.0/apps/${appId}/slots/production/predict?${querystring.stringify(queryParams)}`

        // HTTP Request
        const response = await requestpromise(URI);

        // HTTP Response
        console.log(response);

    }

    // Pass an utterance to the sample LUIS app
    getPrediction().then(()=>console.log("done")).catch((err)=>console.log(err));
    ```

1. `YOUR-KEY` ve `YOUR-ENDPOINT` değerlerini kendi tahmin anahtarınızla ve uç noktanızla değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter tahmini anahtarınız.|
    |`YOUR-ENDPOINT`| Tahmin URL 'niz uç noktasıdır. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Bu komutla `request`, `request-promise`ve `querystring` bağımlılıklarını yükler:

    ```console
    npm install request request-promise querystring
    ```

1. Uygulamanızı şu komutla çalıştırın:

    ```console
    node predict.js
    ```

 1. JSON olarak döndürülen tahmin yanıtını gözden geçirin:

    ```console
    {"query":"turn on all lights","prediction":{"topIntent":"HomeAutomation.TurnOn","intents":{"HomeAutomation.TurnOn":{"score":0.5375382},"None":{"score":0.08687421},"HomeAutomation.TurnOff":{"score":0.0207554}},"entities":{"HomeAutomation.Operation":["on"],"$instance":{"HomeAutomation.Operation":[{"type":"HomeAutomation.Operation","text":"on","startIndex":5,"length":2,"score":0.724984169,"modelTypeId":-1,"modelType":"Unknown","recognitionSources":["model"]}]}}}}
    ```

    Okunabilirlik için biçimlendirilen JSON yanıtı:

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