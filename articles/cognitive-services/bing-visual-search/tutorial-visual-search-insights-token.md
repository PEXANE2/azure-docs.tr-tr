---
title: ImageInsightsToken - Bing Visual Search'u kullanarak önceki aramalardan benzer görüntüleri bulun
titleSuffix: Azure Cognitive Services
description: ImageInsightsToken tarafından belirtilen görüntülerin URL'lerini almak için Bing Görsel Arama SDK'sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: aahi
ms.openlocfilehash: d005800ed317ff21389f18e4440858ea11042e53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370089"
---
# <a name="tutorial-find-similar-images-from-previous-searches-using-an-image-insights-token"></a>Öğretici: Görüntü öngörüleri belirteci kullanarak önceki aramalardan benzer görüntüleri bulma

Görsel Arama SDK, önceki aramalardan çevrimiçi olarak bir `ImageInsightsToken`. Bu uygulama `ImageInsightsToken` bir alır ve sonraki bir aramada belirteci kullanır. Daha sonra `ImageInsightsToken` Bing'e gönderir ve çevrimiçi olarak bulunan benzer görüntülerin Bing Arama URL'leri ve URL'lerini içeren sonuçları döndürür.

Bu öğretici için tam kaynak kodu [github](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInsightsTokens.cs)ek hata işleme ve ek açıklamalar ile bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* Linux/MacOS kullanıyorsanız, bu uygulamayı [Mono](https://www.mono-project.com/)kullanarak çalıştırabilirsiniz.
* NuGet Görsel Arama ve Resim Arama paketleri.
    - Visual Studio'daki Solution Explorer'dan projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet'i** seçin. Paketi `Microsoft.Azure.CognitiveServices.Search.CustomSearch` ve paketi `Microsoft.Azure.CognitiveServices.Search.ImageSearch` yükleyin. NuGet paketlerinin yüklenmesi de aşağıdakileri yükler:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-sdk"></a>Bing Resim Arama SDK'dan ImageInsightsToken'ı edinin

Bu uygulama `ImageInsightsToken` [Bing Resim Arama SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart)aracılığıyla elde edilen kullanır. Yeni bir C# konsol uygulamasında, API'yi `ImageSearchClient()`kullanmak için bir istemci oluşturun. Sonra `SearchAsync()` sorgu ile kullanın:

```csharp
var client = new ImageSearchClient(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

İlk arama sonucunu `imageResults.Value.First()`kullanarak depolayın ve ardından `ImageInsightsToken`görüntü öngörüsün'ünkini saklayın.

```csharp
String insightTok = "None";
if (imageResults.Value.Count > 0)
{
    var firstImageResult = imageResults.Value.First();
    insightTok = firstImageResult.ImageInsightsToken;
}
else
{
    insightTok = "None found";
    Console.WriteLine("Couldn't find image results!");
}
```

Bu, `ImageInsightsToken` bing Görsel Arama'ya bir istekle gönderilir.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>VisualInsightsToken'ı Görsel Arama isteğine ekleme

`ImageInsightsToken` Bing Görsel Arama'dan `ImageInsightsToken` alınan yanıtlarda bulunan bir `ImageInfo` nesne oluşturarak Görsel Arama isteğini belirtin.

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>ImageInsightsToken'deki görüntüleri bulmak için Bing Görsel Arama'yı kullanma

Nesne `VisualSearchRequest` aranacak resim `ImageInfo` hakkında bilgi içerir. `VisualSearchMethodAsync()` yöntemi sonuçları alır. Görüntü belirteç tarafından temsil edildiğinden, görüntü ikili sağlamak zorunda değilsiniz.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Görsel Arama sonuçlarına göre yineleyin

Görsel Arama sonuçları `ImageTag` nesneleridir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her `ImageAction` biri, eylem türüne bağlı değerlerin bir listesi olan bir `Data` alan içerir. Örneğin, `ImageTag` nesneler aracılığıyla yineleyebilir ve `ImageAction` etiketi içinde alabilirsiniz. `visualSearchResults.Tags` Aşağıdaki örnekte `PagesIncluding` eylemlerin ayrıntıları yazdırılır:

```csharp
if (visualSearchResults.Tags.Count > 0)
{
    // List of tags
    foreach (ImageTag t in visualSearchResults.Tags)
    {
        foreach (ImageAction i in t.Actions)
        {
            Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

            if (i.ActionType == "PagesIncluding")
            {
                foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                {
                    Console.WriteLine("ContentURL: " + o.ContentUrl);
                }
            }
        }
    }
}
```

### <a name="pagesincluding-actiontypes"></a>ActionTypes Dahil Sayfalar

Eylem türlerinden gerçek görüntü URL'leri almak, `ActionType` değer `ImageModuleAction`listesi içeren `Data` bir öğe içeren bir "olarak" okuyan bir döküm gerektirir. Her değer, bir görüntünün URL’sidir.  Aşağıdaki `PagesIncluding` eylem türünü atar `ImageModuleAction` ve değerleri okur:

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

Bu veri türleri hakkında daha fazla bilgi için bkz. [Görüntüler - Görsel Arama](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).

## <a name="returned-urls"></a>Döndürülen URL'ler

Uygulamanın tamamı aşağıdaki URL'leri döndürür:

|EylemTürü  |URL'si  | |
|---------|---------|---------|
|MoreSizes -> WebSearchUrl     |         |
|VisualSearch -> WebSearchUrl     |         |
|ImageById -> WebSearchUrl    |         |
|İlgiliAramalar -> WebSearchUrl:    |         |
|DocumentLevelSuggestions -> WebSearchUrl:     |         |
|Konu Sonuçları -> WebSearchUrl    | https:\//www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kz q1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1       |
|ImageResults -> WebSearchUrl    |  https:\//www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSD RERE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1       |

Yukarıda gösterildiği `TopicResults` gibi, `ImageResults` ve türleri ilgili görüntüler için sorguları içerir. URL'ler Bing arama sonuçlarına bağlantı verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalık web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
