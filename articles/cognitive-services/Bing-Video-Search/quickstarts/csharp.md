---
title: 'Hızlı başlangıç: REST API ve C# Bing video arama kullanarak video arama'
titleSuffix: Azure Cognitive Services
description: C# kullanarak Bing Video Arama REST API video arama istekleri göndermek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 10/22/2020
ms.author: clschott
ms.custom: devx-track-csharp
ms.openlocfilehash: 24e172edd47bd859e530536671b5e8df7e0d4c8f
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96352567"
---
# <a name="quickstart-search-for-videos-using-the-bing-video-search-rest-api-and-c"></a>Hızlı başlangıç: Bing Video Arama REST API ve C kullanarak video arayın #

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Video Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu basit C# uygulaması, API 'ye bir HTTP video arama sorgusu gönderir ve JSON yanıtını görüntüler. Bu uygulama C# dilinde yazılsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Bu örneğe ilişkin kaynak kodu, [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingVideoSearchv7.cs) ek hata işleme, Özellikler ve kod ek açıklamalarıyla kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Makinenizi .NET Core çalıştıracak şekilde ayarlamanız gerekir. Yükleme yönergelerini [.NET Core İndirmeleri](https://dotnet.microsoft.com/download) sayfasında bulabilirsiniz. Bu uygulamayı Windows, Linux, macOS veya Docker kapsayıcısında çalıştırabilirsiniz. En sevdiğiniz kod düzenleyicinizi yüklemeniz gerekir. Aşağıdaki açıklamalar açık kaynaklı, platformlar arası bir düzenleyici olan [Visual Studio Code](https://code.visualstudio.com/)kullanır. Bununla birlikte, rahat olan her türlü aracı kullanabilirsiniz.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

İlk adım yeni bir uygulama oluşturmaktır. Bir komut istemi açın ve uygulamanız için yeni bir dizin oluşturun. Geçerli dizini oluşturun. Konsol penceresine aşağıdaki komutu girin:

```dotnetcli
dotnet new console --name VideoSearchClient
```

`using`C# derleyicisinin görevi ve JSON türlerini tanıması için aşağıdaki yönergeyi Main yönteminizin en üstüne eklemeniz gerekir:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using System.Collections.Generic;
using System.Text.Json;
using System.Text.Json.Serialization;
```

Abonelik anahtarınız, uç noktanız ve arama teriminiz için değişken ekleyin. Değer için `uriBase` aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure Portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

```csharp
// Replace the accessKey string value with your valid access key.
const string _accessKey = "enter your key here";

// Or use the custom subdomain endpoint displayed in the Azure portal for your resource.
const string _uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search";

const string _searchTerm = "kittens";
```

Sonra, zaman uyumsuz yöntemleri kullanabilmeniz için Main metodunu güncelleştirin. Zaman uyumsuz değiştiricisini ekleyin ve dönüş türünü görev olarak değiştirin.

```csharp
static async Task Main(string[] args)
{
    
}
```

Şimdi, hiçbir şey yapmaz ancak zaman uyumsuz olarak bu programa sahip olursunuz. Bunu geliştirelim.

## <a name="create-a-data-structure-to-hold-the-bing-video-search-api-response"></a>Bing Video Arama API'si yanıtını tutacak bir veri yapısı oluşturma

`SearchResult` `Video` Video arama sonuçlarını içerecek bir ve sınıfı tanımlayın. Daha sonra JSON sonucundan diğer alanlara ihtiyacınız olduğunda daha fazla özellik ekleyebilirsiniz.

```cscharp
class SearchResult
{
    [JsonPropertyName("totalEstimatedMatches")]
    public int TotalEstimatedMatches { get; set; }

    [JsonPropertyName("value")]
    public List<Video> Videos { get; set; }
}

class Video
{
    [JsonPropertyName("name")]
    public string Name { get; set; }

    [JsonPropertyName("description")]
    public string Description { get; set; }

    [JsonPropertyName("thumbnailUrl")]
    public string ThumbnailUrl { get; set; }

    [JsonPropertyName("contentUrl")]
    public string ContentUrl { get; set; }
}
```

## <a name="create-and-handle-a-video-search-request"></a>Video arama isteği oluşturma ve işleme

`HttpClient`API 'ye çağrı yapmak için kullanıyoruz. İlk olarak, üstbilgiyi `Ocp-Apim-Subscription-Key` ve erişim anahtarınızı eklememiz gerekiyor. 

```csharp
using var client = new HttpClient();
client.BaseAddress = new Uri(_uriBase);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _accessKey);
```

Arama isteği için URI oluşturun. `_searchTerm`Dizeye eklemeden önce arama terimini biçimlendirin.

```csharp
var response = await client.GetAsync($"?q={Uri.EscapeDataString(_searchTerm)}");
```

## <a name="process-the-result"></a>Sonucu işleme

Yanıt başarıyla tamamlandığında JSON verilerini işleyebiliriz. JSON dizesinin serisini `SearchResult` daha önce oluşturduğumuzdan çıkardık. Sonucun (varsa) döngüsünü yapın ve sonucu konsola yazdırın.

```csharp
if (response.IsSuccessStatusCode)
{
    var json = await response.Content.ReadAsStringAsync();
    var result = JsonSerializer.Deserialize<SearchResult>(json);

    foreach (var video in result.Videos)
    {
        Console.WriteLine($"Name: {video.Name}");
        Console.WriteLine($"ContentUrl: {video.ContentUrl}");
        Console.WriteLine();
    }
}
```

## <a name="example-json-response"></a>Örnek JSON yanıtı 

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
    "_type": "Videos",
    "instrumentation": {},
    "readLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?q=kittens",
    "webSearchUrl": "https://www.bing.com/videos/search?q=kittens",
    "totalEstimatedMatches": 1000,
    "value": [
        {
            "webSearchUrl": "https://www.bing.com/videos/search?q=kittens&view=...",
            "name": "Top 10 cute kitten videos compilation",
            "description": "HELP HOMELESS ANIMALS AND WIN A PRIZE BY CHOOSING...",
            "thumbnailUrl": "https://tse4.mm.bing.net/th?id=OVP.n1aE_Oikl4MtzBb...",
            "datePublished": "2014-11-12T22:47:36.0000000",
            "publisher": [
                {
                    "name": "Fabrikam"
                }
            ],
            "creator": {
                "name": "Marcus Appel"
            },
            "isAccessibleForFree": true,
            "contentUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "hostPageUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "encodingFormat": "h264",
            "hostPageDisplayUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "width": 480,
            "height": 360,
            "duration": "PT3M52S",
            "motionThumbnailUrl": "https://tse4.mm.bing.net/th?id=OM.j4QyJAENJphdZQ_1501386166&pid=Api",
            "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"https://www.fabrikam.com/embed/8HVWitAW-Qg?autoplay=1\" frameborder=\"0\" allowfullscreen></iframe>",
            "allowHttpsEmbed": true,
            "viewCount": 7513633,
            "thumbnail": {
                "width": 300,
                "height": 168
            },
            "videoId": "655D98260D012432848F6558260D012432848F",
            "allowMobileEmbed": true,
            "isSuperfresh": false
        },
        . . .
    ],
    "nextOffset": 36,
    "queryExpansions": [
        {
            "text": "Kittens Meowing",
            "displayText": "Meowing",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Kittens+Meowing...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
            "thumbnail": {
                "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Kittens+Meowing&pid..."
            }
        },
        {
            "text": "Funny Kittens",
            "displayText": "Funny",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Funny+Kittens...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
            "thumbnail": {
                "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Funny+Kittens&..."
            }
        },
        . . .
    ],
    "pivotSuggestions": [
        {
            "pivot": "kittens",
            "suggestions": [
                {
                    "text": "Cat",
                    "displayText": "Cat",
                    "webSearchUrl": "https://www.bing.com/videos/search?q=Cat...",
                    "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?...",
                    "thumbnail": {
                        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Cat&pid=Api..."
                    }
                },
                {
                    "text": "Feral Cat",
                    "displayText": "Feral Cat",
                    "webSearchUrl": "https://www.bing.com/videos/search?q=Feral+Cat...",
                    "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
                    "thumbnail": {
                        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Feral+Cat&pid=Api&..."
                    }
                }
            ]
        }
    ],
    "relatedSearches": [
        {
            "text": "Kittens Being Born",
            "displayText": "Kittens Being Born",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Kittens+Being+Born...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?...",
            "thumbnail": {
                "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Kittens+Being+Born&pid=..."
            }
        },
        . . .
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Bing Video Arama API'si nedir?](../overview.md)