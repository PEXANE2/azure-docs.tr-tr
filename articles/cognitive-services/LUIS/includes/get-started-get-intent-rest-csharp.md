---
title: REST çağrısı ile tahmin alC#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: 4cbec342bc20de35c0c62284e4e1fe1ae8b8e8a4
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76966723"
---
## <a name="prerequisites"></a>Ön koşullar

* [.NET Core V 2.2 +](https://dotnet.microsoft.com/download)
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

C# [Tahmin uç noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucu almak için (.NET Core) kullanın.

1. `predict-with-rest`bir proje ve klasör adı ile C# dili hedefleyen yeni bir konsol uygulaması oluşturun.

    ```console
    dotnet new console -lang C# -n predict-with-rest
    ```

1. Yeni oluşturduğunuz `predict-with-rest` dizinine geçin ve gerekli bağımlılıkları şu komutlarla birlikte yüklemelisiniz:

    ```console
    cd predict-with-rest
    dotnet add package System.Net.Http
    ```

1. En sevdiğiniz IDE veya düzenleyicide `Program.cs` açın. Ardından aşağıdaki kodla `Program.cs` üzerine yazın:

   ```csharp
    using System;
    using System.Net.Http;
    using System.Web;

    namespace predict_with_rest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // YOUR-KEY: 32 character key
                var key = "YOUR-KEY";

                // YOUR-ENDPOINT: example is your-resource-name.api.cognitive.microsoft.com
                var endpoint = "YOUR-ENDPOINT";

                // //public sample app
                var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

                var utterance = "turn on all lights";

                MakeRequest(key, endpoint, appId, utterance);

                Console.WriteLine("Hit ENTER to exit...");
                Console.ReadLine();
            }
            static async void MakeRequest(string key, string endpoint, string appId, string utterance)
            {
                var client = new HttpClient();
                var queryString = HttpUtility.ParseQueryString(string.Empty);

                // The request header contains your subscription key
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

                // The "q" parameter contains the utterance to send to LUIS
                queryString["query"] = utterance;

                // These optional request parameters are set to their default values
                queryString["verbose"] = "true";
                queryString["show-all-intents"] = "true";
                queryString["staging"] = "false";
                queryString["timezoneOffset"] = "0";

                var endpointUri = String.Format("https://{0}/luis/prediction/v3.0/apps/{1}/slots/production/predict?query={2}", endpoint, appId, queryString);

                var response = await client.GetAsync(endpointUri);

                var strResponseContent = await response.Content.ReadAsStringAsync();

                // Display the JSON result from LUIS
                Console.WriteLine(strResponseContent.ToString());
            }
        }
    }

   ```

1. `YOUR-KEY` ve `YOUR-ENDPOINT` değerlerini kendi tahmin anahtarınızla ve uç noktanızla değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter tahmini anahtarınız.|
    |`YOUR-ENDPOINT`| Tahmin URL 'niz uç noktasıdır. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Aşağıdaki komutla konsol uygulaması oluşturun:

    ```console
    dotnet build
    ```

1. Konsol uygulamasını çalıştırın. Konsol çıktısı, daha önce tarayıcı penceresinde gördüğünüz JSON 'ı görüntüler.

    ```console
    dotnet run
    ```

1. JSON olarak döndürülen tahmin yanıtını gözden geçirin:

    ```console
    Hit ENTER to exit...
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
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