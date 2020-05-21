---
title: "C 'de REST çağrısıyla model al #"
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: b1bf3c0d7c902a048881b5fb75783744214b073e
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655482"
---
## <a name="prerequisites"></a>Ön koşullar

* Azure Language Understanding-yazma kaynağı 32 karakter anahtarı ve yazma uç noktası URL 'SI. [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) veya [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli)ile oluşturun.
* GitHub deposundan [pizza](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json) uygulamasını içeri aktarın `Azure-Samples/cognitive-services-sample-data-files` .
* Alınan pizza uygulaması için LUSıS uygulama KIMLIĞI. Uygulama kimliği, uygulama panosunda gösterilir.
* Konuşmaları alan uygulamanın içindeki sürüm kimliği.
* [.NET Core 3,1](https://dotnet.microsoft.com/download)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="change-model-programmatically"></a>Modeli program aracılığıyla değiştirme

1. Proje ve klasör adı ile C# dilini hedefleyen yeni bir konsol uygulaması oluşturun `csharp-model-with-rest` .

    ```console
    dotnet new console -lang C# -n csharp-model-with-rest
    ```

1. `csharp-model-with-rest`Oluşturduğunuz dizine geçin ve gerekli bağımlılıkları şu komutlarla birlikte yükleyebilirsiniz:

    ```console
    cd csharp-model-with-rest
    dotnet add package System.Net.Http
    dotnet add package JsonFormatterPlus
    ```

1. Program.cs içeriğini şu kodla değiştirin:

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using System.Linq;

    // 3rd party NuGet packages
    using JsonFormatterPlus;

    namespace AddUtterances
    {
        class Program
        {
            // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
            static string appID = "YOUR-APP-ID";

            // YOUR-AUTHORING-KEY: Your LUIS authoring key, 32 character value.
            static string authoringKey = "YOUR-AUTHORING-KEY";

            // YOUR-AUTHORING-ENDPOINT: Replace this endpoint with your authoring key endpoint.
            // For example, "https://your-resource-name.api.cognitive.microsoft.com/"
            static string endpoint = "YOUR-AUTHORING-ENDPOINT";

            // NOTE: Replace this your version number.
            static string appVersion = "0.1";

            static string host = String.Format("{0}luis/authoring/v3.0-preview/apps/{1}/versions/{2}/", endpoint, appID, appVersion);

            // GET request with authentication
            async static Task<HttpResponseMessage> SendGet(string uri)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Get;
                    request.RequestUri = new Uri(uri);
                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }

            // POST request with authentication
            async static Task<HttpResponseMessage> SendPost(string uri, string requestBody)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Post;
                    request.RequestUri = new Uri(uri);

                    if (!String.IsNullOrEmpty(requestBody))
                    {
                        request.Content = new StringContent(requestBody, Encoding.UTF8, "text/json");
                    }

                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }

            // Add utterances as string with POST request
            async static Task AddUtterances(string utterances)
            {
                string uri = host + "examples";

                var response = await SendPost(uri, utterances);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Added utterances.");
                Console.WriteLine(JsonFormatter.Format(result));
            }

            // Train app after adding utterances
            async static Task Train()
            {
                string uri = host  + "train";

                var response = await SendPost(uri, null);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Sent training request.");
                Console.WriteLine(JsonFormatter.Format(result));
            }

            // Check status of training
            async static Task Status()
            {
                var response = await SendGet(host  + "train");
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Requested training status.");
                Console.WriteLine(JsonFormatter.Format(result));
            }

            // Add utterances, train, check status
            static void Main(string[] args)
            {
                string utterances = @"
                [
                    {
                        'text': 'order a pizza',
                        'intentName': 'ModifyOrder',
                        'entityLabels': [
                            {
                                'entityName': 'Order',
                                'startCharIndex': 6,
                                'endCharIndex': 12
                            }
                        ]
                    },
                    {
                        'text': 'order a large pepperoni pizza',
                        'intentName': 'ModifyOrder',
                        'entityLabels': [
                            {
                                'entityName': 'Order',
                                'startCharIndex': 6,
                                'endCharIndex': 28
                            },
                            {
                                'entityName': 'FullPizzaWithModifiers',
                                'startCharIndex': 6,
                                'endCharIndex': 28
                            },
                            {
                                'entityName': 'PizzaType',
                                'startCharIndex': 14,
                                'endCharIndex': 28
                            },
                            {
                                'entityName': 'Size',
                                'startCharIndex': 8,
                                'endCharIndex': 12
                            }
                        ]
                    },
                    {
                        'text': 'I want two large pepperoni pizzas on thin crust',
                        'intentName': 'ModifyOrder',
                        'entityLabels': [
                            {
                                'entityName': 'Order',
                                'startCharIndex': 7,
                                'endCharIndex': 46
                            },
                            {
                                'entityName': 'FullPizzaWithModifiers',
                                'startCharIndex': 7,
                                'endCharIndex': 46
                            },
                            {
                                'entityName': 'PizzaType',
                                'startCharIndex': 17,
                                'endCharIndex': 32
                            },
                            {
                                'entityName': 'Size',
                                'startCharIndex': 11,
                                'endCharIndex': 15
                            },
                            {
                                'entityName': 'Quantity',
                                'startCharIndex': 7,
                                'endCharIndex': 9
                            },
                            {
                                'entityName': 'Crust',
                                'startCharIndex': 37,
                                'endCharIndex': 46
                            }
                        ]
                    }
                ]
                ";

                AddUtterances(utterances).Wait();
                Train().Wait();
                Status().Wait();
            }
        }
    }
    ```

1. İle başlayan değerleri `YOUR-` kendi değerlerinizle değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-APP-ID`| LUSıS uygulama KIMLIĞINIZ. |
    |`YOUR-AUTHORING-KEY`|32 karakter yazma anahtarınız.|
    |`YOUR-AUTHORING-ENDPOINT`| Yazma URL 'niz uç noktasıdır. Örneğin, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki Luo portalında görünür. Uygulama KIMLIĞI, **uygulama ayarları** sayfasında aynı Yönet bölümünde bulunur.

1. Konsol uygulamasını derleyin.

    ```console
    dotnet build
    ```

1. Konsol uygulamasını çalıştırın. Konsol çıktısı, daha önce tarayıcı penceresinde gördüğünüz JSON 'ı görüntüler.

    ```console
    dotnet run
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, proje klasörünü dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](../luis-concept-best-practices.md)
