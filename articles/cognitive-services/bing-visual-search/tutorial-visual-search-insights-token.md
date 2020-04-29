---
title: Image Insights belirteçlerini ve Bing Görsel Arama API'si kullanarak önceki aramalardan benzer görüntüleri bulun
titleSuffix: Azure Cognitive Services
description: Önceki aramalardan görüntülerin URL 'Lerini almak için Bing Görsel Arama istemci kitaplığını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/31/2020
ms.author: aahi
ms.openlocfilehash: ad24a8a194a11c3fd5f7f77ea8c52197d5438edc
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80477913"
---
# <a name="tutorial-find-similar-images-from-previous-searches-using-an-image-insights-token"></a>Öğretici: görüntü öngörüleri belirtecini kullanarak önceki aramalardan benzer görüntüleri bulma

Görsel Arama istemci kitaplığı, döndüren önceki aramalardan resimleri çevrimiçi olarak bulmanızı sağlar `ImageInsightsToken`. Bu uygulama bir `ImageInsightsToken` alır ve sonraki aramada belirtecini kullanır. Daha sonra bunu Bing `ImageInsightsToken` 'e gönderir ve Bing arama URL 'leri ve çevrimiçi bulunan benzer görüntülerin URL 'lerini içeren sonuçları döndürür.

Bu öğreticinin tam kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInsightsTokens.cs)üzerinde ek hata işleme ve ek açıklamalarla bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Herhangi bir [Visual Studio 2019](https://www.visualstudio.com/downloads/)sürümü.
* Linux/MacOS kullanıyorsanız bu uygulamayı [mono](https://www.mono-project.com/)kullanarak çalıştırabilirsiniz.
* NuGet Görsel Arama ve Resim Arama paketleri.
    - Visual Studio 'daki Çözüm Gezgini projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet** ' i seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` Paketini ve `Microsoft.Azure.CognitiveServices.Search.ImageSearch` paketini yükler. NuGet paketlerini yüklemek aşağıdakileri de yapar:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-client-library"></a>Bing Resim Arama istemci kitaplığından ımageınsibir sToken alın

Bu uygulama, `ImageInsightsToken` [Bing resim arama istemci kitaplığı](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart)aracılığıyla elde edilen bir kullanır. Yeni bir C# konsol uygulamasında, kullanarak `ImageSearchClient()`API 'yi çağırmak için bir istemci oluşturun. Sonra Sorgunuzla `SearchAsync()` birlikte kullanın:

```csharp
var client = new ImageSearchClient(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

' Yi kullanarak `imageResults.Value.First()`ilk arama sonucunu depolayın ve ardından görüntü öngörülerini `ImageInsightsToken`saklayın.

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

Bu `ImageInsightsToken` , bir istekte Bing Görsel Arama gönderilir.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>Görsel Arama isteğine ımageınsibir sToken ekleyin

Bing Görsel Arama yanıtlarındaki yanıtlardan `ImageInsightsToken` `ImageInfo` `ImageInsightsToken` bulunan öğesinden bir nesne oluşturarak görsel arama isteği için belirtin.

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Imageınsilım sToken görüntülerini bulmak için Bing Görsel Arama kullanma

`VisualSearchRequest` Nesnesi, aranacağı içindeki `ImageInfo` görüntüyle ilgili bilgiler içerir. `VisualSearchMethodAsync()` yöntemi sonuçları alır. Görüntü, belirteç tarafından temsil edildiği için bir görüntü ikilisi sağlamanız gerekmez.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Görsel Arama sonuçları boyunca yineleme

Görsel Arama sonuçları `ImageTag` nesneleridir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her `ImageAction` biri, `Data` eylem türüne bağlı değerlerin listesi olan bir alan içerir. İçindeki `ImageTag` `visualSearchResults.Tags`nesneler üzerinde yineleyebilirsiniz ve içindeki `ImageAction` etiketi alabilirsiniz. Aşağıdaki örnekte `PagesIncluding` eylemlerin ayrıntıları yazdırılır:

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

### <a name="pagesincluding-actiontypes"></a>ActionTypes dahil Pagestypes

Eylem türlerinden gerçek görüntü URL 'Lerinin alınması, bir değer listesi içeren bir `ActionType` `ImageModuleAction` `Data` öğe içeren bir olarak okuyan bir tür dönüştürme gerektirir. Her değer, bir görüntünün URL’sidir.  Aşağıdaki `PagesIncluding` eylem türünü öğesine `ImageModuleAction` yayınlar ve değerleri okur:

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

## <a name="returned-urls"></a>Döndürülen URL 'Ler

Uygulamanın tamamı aşağıdaki URL 'Leri döndürür:

|EylemTürü  |URL'si  | |
|---------|---------|---------|
|Moyeniden boyutlanıyor-> WebSearchUrl     |         |
|VisualSearch-> WebSearchUrl     |         |
|Imagebyıd-> WebSearchUrl    |         |
|Relatedaramalarını-> WebSearchUrl:    |         |
|Documentlevelönerilere-> WebSearchUrl:     |         |
|TopicResults-> WebSearchUrl    | https:\//www.BING.com/CR?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CıD = 1BA795A21EAF6A63175699B71FC36B7C&RD = 1&h = BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v = 1&r = https %3 a %2 f %2 f www. Bing. com% 2fdiscover% 2fkanada %2 brocky&p = devex, 5823.1       |
|Imageresults-> WebSearchUrl    |  https:\//www.BING.com/CR?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CıD = 1BA795A21EAF6A63175699B71FC36B7C&RD = 1&h = PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v = 1&r = https %3 a %2 f %2 f www. Bing. com% 2fimages% 2fsearch% 3Fq% 3doutdoor&p = devex, 5831.1       |

Yukarıda gösterildiği gibi, `TopicResults` ve `ImageResults` türleri ilgili görüntüler için sorgular içerir. URL 'Ler Bing arama sonuçlarına bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
