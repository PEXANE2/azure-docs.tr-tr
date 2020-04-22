---
title: "C'deki REST araması ile tahmin de bulunun #"
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/20/2020
ms.author: diberry
ms.openlocfilehash: 20916ff80ae52ee9fc215d87c0987900d89e590a
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733298"
---
## <a name="prerequisites"></a>Ön koşullar

* [.NET Çekirdek V2.2+](https://dotnet.microsoft.com/download)
* [Visual Studio Code](https://code.visualstudio.com/)
* Bir LUIS uygulama kimliği - genel IoT uygulama kimliğini `df67dcdb-c37d-46af-88e1-8b97951ca1c2`kullanın. Quickstart kodunda kullanılan kullanıcı sorgusu bu uygulamaya özgüdür.

## <a name="create-luis-runtime-key-for-predictions"></a>Öngörüler için LUIS çalışma zamanı anahtarı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açma
1. [Dil **Anlama Oluştur'u** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) tıklatın
1. Runtime tuşu için gerekli tüm ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenilen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`- minimum fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçme|

1. **Oluştur'u** tıklatın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin.
1. Yapılandırılmış `endpoint` ve bir `key`toplamak .

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

[Tahmin bitiş noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucunu almak için C# (.NET Core) kullanın.

1. C# dilini hedefleyen, proje ve klasör adı ile yeni `predict-with-rest`bir konsol uygulaması oluşturun.

    ```console
    dotnet new console -lang C# -n predict-with-rest
    ```

1. Yeni oluşturduğunuz dizine `predict-with-rest` değiştirin ve bu komutlarla gerekli bağımlılıkları yükleyin:

    ```console
    cd predict-with-rest
    dotnet add package System.Net.Http
    ```

1. En `Program.cs` sevdiğiniz IDE veya düzenleyicide açın. Sonra aşağıdaki `Program.cs` kod ile üzerine yazın:

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

1. `YOUR-KEY` Ve `YOUR-ENDPOINT` değerleri kendi tahmin anahtarınız ve bitiş noktanızla değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter tahmin anahtarınız.|
    |`YOUR-ENDPOINT`| Tahmin URL bitiş noktanız. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Bu komutla konsol uygulamasını oluşturun:

    ```console
    dotnet build
    ```

1. Konsol uygulamasını çalıştırın. Konsol çıkışı, tarayıcı penceresinde daha önce gördüğünüz JSON'ı görüntüler.

    ```console
    dotnet run
    ```

1. JSON olarak döndürülen tahmin yanıtını gözden geçirin:

    ```console
    Hit ENTER to exit...
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    JSON yanıtı okunabilirlik için biçimlendirilmiş:

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