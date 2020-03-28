---
title: Arama sonuçlarını görüntülemek için sıralamayı kullanma
titleSuffix: Azure Cognitive Services
description: Arama sonuçlarını sıralama sırasına göre görüntülemek için Bing RankingResponse yanıtının nasıl kullanılacağını gösterir.
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75382336"
---
# <a name="build-a-console-app-search-client-in-c"></a>C'de konsol uygulaması arama istemcisi oluşturma #

Bu öğretici, kullanıcıların Bing Web Arama API'sini sorgulamasına ve sıralanmış sonuçları görüntülemesine olanak tanıyan basit bir .NET Core konsol uygulamasının nasıl oluşturulabildiğini gösterir.

Bu öğretici nasıl gösterin:

- Bing Web Arama API'sine basit bir sorgu yapma
- Sıralı sırada sorgu sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Öğretici ile birlikte takip etmek için, ihtiyacınız var:

- Visual Studio. Eğer yoksa, [indirin ve ücretsiz Visual Studio 2017 Community Edition yükleyin.](https://www.visualstudio.com/downloads/)
- Bing Web Arama API'si için bir abonelik anahtarı. Bir aboneliğiniz yoksa [ücretsiz deneme sürümüne kaydolun](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

## <a name="create-a-new-console-app-project"></a>Yeni bir Konsol Uygulaması projesi oluşturun

Visual Studio’da `Ctrl`+`Shift`+`N` ile yeni bir proje oluşturun.

Yeni **Proje** iletişim kutusunda Visual **C# > Windows Classic Desktop > Console App (.NET Framework)** seçeneğini tıklayın.

Uygulama **MyConsoleSearchApp**adı ve sonra **Tamam'ı**tıklatın.

## <a name="add-the-jsonnet-nuget-package-to-the-project"></a>projeye JSON.net Nuget paketini ekleyin

JSON.net, API tarafından döndürülen JSON yanıtlarıyla çalışmanızı sağlar. NuGet paketini projenize ekleyin:

- **Solution Explorer'da** projeye sağ tıklayın ve **NuGet Paketlerini Yönet'i seçin...**.
- **Gözat** sekmesinde, ' `Newtonsoft.Json`ı arayın. En son sürümü seçin ve sonra **Yükle'yi**tıklatın.
- **Değişiklikleri İncele** **penceresindetamam** düğmesini tıklatın.
- **NuGet: MyConsoleSearchApp**başlıklı Visual Studio sekmesini kapatın.

## <a name="add-a-reference-to-systemweb"></a>System.Web'e başvuru ekleme

Bu öğretici `System.Web` derlemeye dayanır. Projenize bu derlemeye bir başvuru ekleyin:

- **Solution**Explorer'da, **Referanslar'a** sağ tıklayın ve **Referans Ekle'yi seçin...**
- **Çerçeve > Derlemeler'i**seçin, ardından aşağı kaydırın ve **System.Web'i** denetleyin
- **Tamam'ı** seçin

## <a name="add-some-necessary-using-statements"></a>Bazı gerekli ifadeleri kullanarak ekleme

Bu öğreticideki kod, üç ek kullanma deyimleri gerektirir. bu ifadeleri **Program.cs** `using` üstündeki varolan ifadelerin altına ekleyin:

```csharp
using System.Web;
using System.Net.Http;
```

## <a name="ask-the-user-for-a-query"></a>Kullanıcıdan sorgu isteyin

**Çözüm Gezgini'nde,** **Program.cs**açın. Yöntemi `Main()` güncelleştirin:

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

- Kullanıcıdan sorgu ister
- Sorguyu yürütmek ve sonuçları görüntülemek için çağrılar `RunQueryAndDisplayResults(userQuery)`
- Konsol penceresinin hemen kapanmasını önlemek için kullanıcı girişi bekler.

## <a name="search-for-query-results-using-the-bing-web-search-api"></a>Bing Web Arama API'sini kullanarak sorgu sonuçlarını arama

Ardından, API'yi sorgulayan ve sonuçları görüntüleyen bir yöntem ekleyin:

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

- Web Arama `HttpClient` API'sini sorgulamak için bir oluşturma
- Bing'in `Ocp-Apim-Subscription-Key` isteği doğrulamak için kullandığı HTTP üstbilgisini ayarlar
- İsteği yürütür ve sonuçları deserialize etmek için JSON.net kullanır
- Tüm `DisplayAllRankedResults(responseObjects)` sonuçları sıralı sırada görüntülemek için yapılan aramalar

Abonelik anahtarınızın değerini `Ocp-Apim-Subscription-Key` ayarladıklarından emin olun.

## <a name="display-ranked-results"></a>Sıralamalı sonuçları görüntüleme

Sonuçların sıralı sırada nasıl görüntülenilebildiğini göstermeden önce, örnek bir web arama yanıtına bir göz atın:

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

`rankingResponse` JSON[nesnesi (belgeler)](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse)arama sonuçları için uygun görüntü sırasını açıklar. Aşağıdaki, öncelikli gruplardan birini veya birkaçını içerir:

- `pole`: En görünür tedaviyi elde etmek için arama sonuçları (örneğin, ana çizgi ve kenar çubuğunun üzerinde görüntülenir).
- `mainline`: Ana satırda görüntülenecek arama sonuçları.
- `sidebar`: Kenar çubuğunda görüntülenecek arama sonuçları. Kenar çubuğu yoksa, sonuçları ana çizginin altında görüntüleyin.

Sıralama yanıtı JSON bir veya daha fazla grupları içerebilir.

**Program.cs,** sonuçları düzgün sıralanmış sırada görüntülemek için aşağıdaki yöntemi ekleyin:

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

- Yanıtın `rankingResponse` içerdiği gruplar üzerinde döngüler
- Her gruptaki öğeleri arayarak görüntüler`DisplaySpecificResults(...)`

**Program.cs**olarak, aşağıdaki iki yöntem ekleyin:

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

Bu yöntemler, arama sonuçlarını konsola çıkarmak için birlikte çalışır.

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

[Sonuçları görüntülemek için sıralamayı kullanma](rank-results.md)hakkında daha fazla bilgi edinin.
