---
title: Arama sonuçlarını göstermek için derece kullanma
titleSuffix: Azure Cognitive Services
description: Arama sonuçlarının derece sırada görüntülenmesi için Bing RankingResponse yanıtının nasıl kullanılacağını gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 2575A80C-FC74-4631-AE5D-8101CF2591D3
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: tutorial
ms.date: 12/19/2019
ms.author: aahi
ms.openlocfilehash: 1c8e0bb136fddeb84dc991e63a761378b38cc470
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75382336"
---
# <a name="build-a-console-app-search-client-in-c"></a>C 'de konsol uygulama arama istemcisi oluşturma #

Bu öğreticide, kullanıcıların Bing Web Araması API'si sorgulamasını ve derecelendirilen sonuçları görüntülemesini sağlayan basit bir .NET Core konsol uygulamasının nasıl oluşturulacağı gösterilmektedir.

Bu öğreticide nasıl yapılacağı gösterilmektedir:

- Bing Web Araması API'si basit bir sorgu yapın
- Sorgu sonuçlarını dereceli sırada görüntüle

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyle birlikte izlemek için şunlar gerekir:

- Visual Studio. Bunu yapmazsanız, [ücretsiz Visual Studio 2017 Community Edition ' ı indirip yükleyin](https://www.visualstudio.com/downloads/).
- Bing Web Araması API'si için bir abonelik anahtarı. Bir aboneliğiniz yoksa [ücretsiz deneme sürümüne kaydolun](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

## <a name="create-a-new-console-app-project"></a>Yeni bir konsol uygulaması projesi oluştur

Visual Studio’da `Ctrl`+`Shift`+`N` ile yeni bir proje oluşturun.

**Yeni proje** iletişim kutusunda, **Visual C# > Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)** seçeneğine tıklayın.

Uygulamayı **Myconsolesearchapp**olarak adlandırın ve ardından **Tamam**' a tıklayın.

## <a name="add-the-jsonnet-nuget-package-to-the-project"></a>JSON.net NuGet paketini projeye ekleyin

JSON.net, API tarafından döndürülen JSON yanıtları ile çalışmanıza olanak sağlar. NuGet paketini projenize ekleyin:

- **Çözüm Gezgini** projeye sağ tıklayıp **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
- **Araştır** sekmesinde için `Newtonsoft.Json`arama yapın. En son sürümü seçin ve ardından **Install**' a tıklayın.
- **Değişiklikleri gözden geçir** penceresinde **Tamam** düğmesine tıklayın.
- **NuGet: MyConsoleSearchApp**başlıklı Visual Studio sekmesini kapatın.

## <a name="add-a-reference-to-systemweb"></a>System. Web 'e başvuru ekleme

Bu öğretici `System.Web` derlemeyi temel alır. Projenize bu derlemeye bir başvuru ekleyin:

- **Çözüm Gezgini**, **Başvurular** ' a sağ tıklayın ve **Başvuru Ekle...** öğesini seçin.
- **Derlemeler > Framework**' ü seçin, ardından aşağı kaydırın ve **System. Web** ' i denetleyin
- **Tamam 'ı** seçin

## <a name="add-some-necessary-using-statements"></a>Bazı gerekli using deyimlerini ekleyin

Bu öğreticideki kod, üç ek using deyimi gerektirir. Bu deyimleri, **program.cs**'in en `using` üstündeki mevcut deyimlerinin altına ekleyin:

```csharp
using System.Web;
using System.Net.Http;
```

## <a name="ask-the-user-for-a-query"></a>Kullanıcıdan bir sorgu isteyin

**Çözüm Gezgini**' de, **program.cs**' yi açın. `Main()` Yöntemi güncelleştirin:

```csharp
static void Main()
{
    // Get the user's query
    Console.Write("Enter Bing query: ");
    string userQuery = Console.ReadLine();
    Console.WriteLine();

    // Run the query and display the results
    RunQueryAndDisplayResults(userQuery);

    // Prevent the console window from closing immediately
    Console.WriteLine("\nHit ENTER to exit...");
    Console.ReadLine();
}
```

Bu yöntem:

- Kullanıcıdan bir sorgu ister
- Sorguyu `RunQueryAndDisplayResults(userQuery)` yürütmek ve sonuçları göstermek için çağrılar
- Konsol penceresinin hemen kapatmasını engellemek için Kullanıcı girişi bekler.

## <a name="search-for-query-results-using-the-bing-web-search-api"></a>Bing Web Araması API'si kullanarak sorgu sonuçları arayın

Sonra, API 'YI sorgulayan ve sonuçları görüntüleyen bir yöntem ekleyin:

```csharp
static void RunQueryAndDisplayResults(string userQuery)
{
    try
    {
        // Create a query
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<YOUR_SUBSCRIPTION_KEY_GOES_HERE>");
        var queryString = HttpUtility.ParseQueryString(string.Empty);
        queryString["q"] = userQuery;
        var query = "https://api.cognitive.microsoft.com/bing/v7.0/search?" + queryString;

        // Run the query
        HttpResponseMessage httpResponseMessage = client.GetAsync(query).Result;

        // Deserialize the response content
        var responseContentString = httpResponseMessage.Content.ReadAsStringAsync().Result;
        Newtonsoft.Json.Linq.JObject responseObjects = Newtonsoft.Json.Linq.JObject.Parse(responseContentString);

        // Handle success and error codes
        if (httpResponseMessage.IsSuccessStatusCode)
        {
            DisplayAllRankedResults(responseObjects);
        }
        else
        {
            Console.WriteLine($"HTTP error status code: {httpResponseMessage.StatusCode.ToString()}");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine(e.Message);
    }
}
```

Bu yöntem:

- Web Araması API `HttpClient` 'sini sorgulamak için bir oluşturur
- Bing 'in `Ocp-Apim-Subscription-Key` isteğin kimliğini doğrulamak için kullandığı http üstbilgisini ayarlar
- İsteği yürütür ve sonuçların serisini kaldırmak için JSON.net kullanır
- Tüm `DisplayAllRankedResults(responseObjects)` sonuçları derecelendirilmiş sırada görüntüleme çağrıları

Değerini `Ocp-Apim-Subscription-Key` abonelik anahtarınıza ayarladığınızdan emin olun.

## <a name="display-ranked-results"></a>Derecelendirilmiş sonuçları görüntüle

Sonuçları derecelendirildi sırada görüntülemeyi göstermeden önce, örnek bir Web araması yanıtına göz atın:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346...",
        "totalEstimatedMatches" : 982000,
        "value" : [{
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
            "name" : "Contoso Sailing Club - Seattle",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/contososailingsea...",
            "snippet" : "Come sail with Contoso in Seattle...",
            "dateLastCrawled" : "2017-04-07T02:25:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#WebPages.6",
            "name" : "Contoso Sailing Lessons - Official Site",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/www.constososailinglessonsseat...",
            "snippet" : "Contoso sailing lessons in Seattle...",
            "dateLastCrawled" : "2017-04-09T14:30:00"
        },

        ...

        ],
        "someResultsRemoved" : true
    },
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#RelatedSearches",
        "value" : [{
            "text" : "sailing lessons",
            "displayText" : "sailing lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346E..."
        }

        ...

        ]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            }

            ...

            ]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}
```

`rankingResponse` JSON nesnesi ([Belgeler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse)) arama sonuçları için uygun görüntüleme sırasını açıklar. Aşağıdaki, önceliklendirilmiş gruplardan bir veya daha fazlasını içerir:

- `pole`: En sık görünen işlemi almak için arama sonuçları (örneğin, ana hat ve kenar çubuğu üzerinde gösterilir).
- `mainline`: Ana satırda görüntülenecek arama sonuçları.
- `sidebar`: Kenar çubuğunda görüntülenecek arama sonuçları. Kenar çubuğu yoksa, sonuçları Mainline altında görüntüleyin.

Sıralama yanıtı JSON, bir veya daha fazla gruptan birini içerebilir.

**Program.cs**' de, sonuçları düzgün bir şekilde derecelendirilen sırada göstermek için aşağıdaki yöntemi ekleyin:

```csharp
static void DisplayAllRankedResults(Newtonsoft.Json.Linq.JObject responseObjects)
{
    string[] rankingGroups = new string[] { "pole", "mainline", "sidebar" };

    // Loop through the ranking groups in priority order
    foreach (string rankingName in rankingGroups)
    {
        Newtonsoft.Json.Linq.JToken rankingResponseItems = responseObjects.SelectToken($"rankingResponse.{rankingName}.items");
        if (rankingResponseItems != null)
        {
            foreach (Newtonsoft.Json.Linq.JObject rankingResponseItem in rankingResponseItems)
            {
                Newtonsoft.Json.Linq.JToken resultIndex;
                rankingResponseItem.TryGetValue("resultIndex", out resultIndex);
                var answerType = rankingResponseItem.Value<string>("answerType");
                switch (answerType)
                {
                    case "WebPages":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("webPages.value"), "WebPage", "name", "url", "displayUrl", "snippet");
                        break;
                    case "News":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("news.value"), "News", "name", "url", "description");
                        break;
                    case "Images":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("images.value"), "Image", "thumbnailUrl");
                        break;
                    case "Videos":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("videos.value"), "Video", "embedHtml");
                        break;
                    case "RelatedSearches":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("relatedSearches.value"), "RelatedSearch", "displayText", "webSearchUrl");
                        break;
                }
            }
        }
    }
}
```

Bu yöntem:

- Yanıtın içerdiği `rankingResponse` gruplar üzerinde döngüler
- Her bir gruptaki öğeleri çağırarak görüntüler`DisplaySpecificResults(...)`

**Program.cs**içinde aşağıdaki iki yöntemi ekleyin:

```csharp
static void DisplaySpecificResults(Newtonsoft.Json.Linq.JToken resultIndex, Newtonsoft.Json.Linq.JToken items, string title, params string[] fields)
{
    if (resultIndex == null)
    {
        foreach (Newtonsoft.Json.Linq.JToken item in items)
        {
            DisplayItem(item, title, fields);
        }
    }
    else
    {
        DisplayItem(items.ElementAt((int)resultIndex), title, fields);
    }
}

static void DisplayItem(Newtonsoft.Json.Linq.JToken item, string title, string[] fields)
{
    Console.WriteLine($"{title}: ");
    foreach( string field in fields )
    {
        Console.WriteLine($"- {field}: {item[field]}");
    }
    Console.WriteLine();
}
```

Bu yöntemler, arama sonuçlarının konsola çıktısını almak için birlikte çalışır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın. Çıktı aşağıdakine benzer görünmelidir:

```
Enter Bing query: sailing lessons seattle

WebPage:
- name: Contoso Sailing Club - Seattle
- url: https://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://contososailingsea....
- snippet: Come sail with Contoso in Seattle...

WebPage:
- name: Contoso Sailing Lessons Seattle - Official Site
- url: http://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://www.constososailinglessonsseat...
- snippet: Contoso sailing lessons in Seattle...

...
```

## <a name="next-steps"></a>Sonraki adımlar

[Sonuçları göstermek için derecelendirmeyi kullanma](rank-results.md)hakkında daha fazla bilgi edinin.
