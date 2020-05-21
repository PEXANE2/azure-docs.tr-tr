---
title: "C 'de REST çağrısıyla tahmin al #"
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: 8ae5bf3790db82d8d96f872d6375e9d3b167bf6d
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654345"
---
## <a name="prerequisites"></a>Ön koşullar

* [.NET Core V 2.2 +](https://dotnet.microsoft.com/download)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-pizza-app"></a>Pizza uygulaması oluşturma

[!INCLUDE [Create pizza app](get-started-get-intent-create-pizza-app.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

[Tahmin uç noktasını](https://aka.ms/luis-apim-v3-prediction) sorgulamak ve tahmin sonucu almak Için C# (.NET Core) kullanın.

1. Proje ve klasör adı ile C# dilini hedefleyen yeni bir konsol uygulaması oluşturun `csharp-predict-with-rest` .

    ```console
    dotnet new console -lang C# -n csharp-predict-with-rest
    ```

1. `csharp-predict-with-rest`Oluşturduğunuz dizine geçin ve gerekli bağımlılığı şu komutla birlikte yüklemelisiniz:

    ```console
    cd csharp-predict-with-rest
    dotnet add package System.Net.Http
    ```

1. En `Program.cs` SEVDIĞINIZ IDE veya düzenleyicide açın. Ardından `Program.cs` aşağıdaki kodla üzerine yazın:

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
                // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
                var appId = "YOUR-APP-ID";

                // YOUR-PREDICTION-KEY: 32 character key.
                var key = "YOUR-PREDICTION-KEY";

                // YOUR-PREDICTION-ENDPOINT: Example is "https://westus.api.cognitive.microsoft.com/"
                var endpoint = "YOUR-PREDICTION-ENDPOINT";

                // An utterance to test the pizza app.
                var utterance = "I want two large pepperoni pizzas on thin crust please";

                MakeRequest(key, endpoint, appId, utterance);

                Console.WriteLine("Press ENTER to exit...");
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

                var endpointUri = String.Format("{0}luis/prediction/v3.0/apps/{1}/slots/production/predict?{2}", endpoint, appId, queryString);

                // Remove these before updating the article.
                Console.WriteLine("endpoint: " + endpoint);
                Console.WriteLine("appId: " + appId);
                Console.WriteLine("queryString: " + queryString);
                Console.WriteLine("endpointUri: " + endpointUri);

                var response = await client.GetAsync(endpointUri);

                var strResponseContent = await response.Content.ReadAsStringAsync();

                // Display the JSON result from LUIS.
                Console.WriteLine(strResponseContent.ToString());
            }
        }
    }
    ```

1. `YOUR-APP-ID`, `YOUR-KEY` Ve `YOUR-ENDPOINT` değerlerini kendi tahmin anahtarınızla ve uç noktanızla değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-APP-ID`|Uygulama KIMLIĞINIZ. Uygulamanızın LUO portalında, uygulama ayarları sayfasında bulunur.
    |`YOUR-PREDICTION-KEY`|32 karakter tahmini anahtarınız. Uygulama için Azure kaynakları sayfasında, LUO portalında bulunur.
    |`YOUR-PREDICTION-ENDPOINT`| Tahmin URL 'niz uç noktasıdır. Uygulama için Azure kaynakları sayfasında, LUO portalında bulunur.<br>Örneğin, `https://westus.api.cognitive.microsoft.com/`.|

1. Aşağıdaki komutla konsol uygulaması oluşturun:

    ```console
    dotnet build
    ```

1. Konsol uygulamasını çalıştırın. Konsol çıktısı, daha önce tarayıcı penceresinde gördüğünüz JSON 'ı görüntüler.

    ```console
    dotnet run
    ```

1. JSON olarak döndürülen tahmin yanıtını gözden geçirin:

    ```json
    {"query":"I want two large pepperoni pizzas on thin crust please","prediction":{"topIntent":"ModifyOrder","intents":{"ModifyOrder":{"score":1.0},"None":{"score":8.55E-09},"Greetings":{"score":1.82222226E-09},"CancelOrder":{"score":1.47272727E-09},"Confirmation":{"score":9.8125E-10}},"entities":{"Order":[{"FullPizzaWithModifiers":[{"PizzaType":["pepperoni pizzas"],"Size":[["Large"]],"Quantity":[2],"Crust":[["Thin"]],"$instance":{"PizzaType":[{"type":"PizzaType","text":"pepperoni pizzas","startIndex":17,"length":16,"score":0.9978157,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Size":[{"type":"SizeList","text":"large","startIndex":11,"length":5,"score":0.9984481,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Quantity":[{"type":"builtin.number","text":"two","startIndex":7,"length":3,"score":0.999770939,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Crust":[{"type":"CrustList","text":"thin crust","startIndex":37,"length":10,"score":0.933985531,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}]}}],"$instance":{"FullPizzaWithModifiers":[{"type":"FullPizzaWithModifiers","text":"two large pepperoni pizzas on thin crust","startIndex":7,"length":40,"score":0.90681237,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}]}}],"ToppingList":[["Pepperoni"]],"$instance":{"Order":[{"type":"Order","text":"two large pepperoni pizzas on thin crust","startIndex":7,"length":40,"score":0.9047088,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"ToppingList":[{"type":"ToppingList","text":"pepperoni","startIndex":17,"length":9,"modelTypeId":5,"modelType":"List Entity Extractor","recognitionSources":["model"]}]}}}}
    ```

    Okunabilirlik için biçimlendirilen JSON yanıtı:

    ```JSON
    {
      "query": "I want two large pepperoni pizzas on thin crust please",
      "prediction": {
        "topIntent": "ModifyOrder",
        "intents": {
          "ModifyOrder": {
            "score": 1
          },
          "None": {
            "score": 8.55e-9
          },
          "Greetings": {
            "score": 1.82222226e-9
          },
          "CancelOrder": {
            "score": 1.47272727e-9
          },
          "Confirmation": {
            "score": 9.8125e-10
          }
        },
        "entities": {
          "Order": [
            {
              "FullPizzaWithModifiers": [
                {
                  "PizzaType": [
                    "pepperoni pizzas"
                  ],
                  "Size": [
                    [
                      "Large"
                    ]
                  ],
                  "Quantity": [
                    2
                  ],
                  "Crust": [
                    [
                      "Thin"
                    ]
                  ],
                  "$instance": {
                    "PizzaType": [
                      {
                        "type": "PizzaType",
                        "text": "pepperoni pizzas",
                        "startIndex": 17,
                        "length": 16,
                        "score": 0.9978157,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Size": [
                      {
                        "type": "SizeList",
                        "text": "large",
                        "startIndex": 11,
                        "length": 5,
                        "score": 0.9984481,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Quantity": [
                      {
                        "type": "builtin.number",
                        "text": "two",
                        "startIndex": 7,
                        "length": 3,
                        "score": 0.999770939,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Crust": [
                      {
                        "type": "CrustList",
                        "text": "thin crust",
                        "startIndex": 37,
                        "length": 10,
                        "score": 0.933985531,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ]
                  }
                }
              ],
              "$instance": {
                "FullPizzaWithModifiers": [
                  {
                    "type": "FullPizzaWithModifiers",
                    "text": "two large pepperoni pizzas on thin crust",
                    "startIndex": 7,
                    "length": 40,
                    "score": 0.90681237,
                    "modelTypeId": 1,
                    "modelType": "Entity Extractor",
                    "recognitionSources": [
                      "model"
                    ]
                  }
                ]
              }
            }
          ],
          "ToppingList": [
            [
              "Pepperoni"
            ]
          ],
          "$instance": {
            "Order": [
              {
                "type": "Order",
                "text": "two large pepperoni pizzas on thin crust",
                "startIndex": 7,
                "length": 40,
                "score": 0.9047088,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                  "model"
                ]
              }
            ],
            "ToppingList": [
              {
                "type": "ToppingList",
                "text": "pepperoni",
                "startIndex": 17,
                "length": 9,
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

Bu hızlı başlangıç ile işiniz bittiğinde, proje klasörünü dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Utterslar ve tren ekleme](../get-started-get-model-rest-apis.md)