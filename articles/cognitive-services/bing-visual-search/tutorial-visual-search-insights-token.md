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
ms.openlocfilehash: a867560edfb66527c09e349328246a8fc24dbbd3
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207252"
---
# <a name="tutorial-find-similar-images-from-previous-searches-using-an-image-insights-token"></a>Öğretici: görüntü öngörüleri belirtecini kullanarak önceki aramalardan benzer görüntüleri bulma

Görsel Arama istemci kitaplığı, döndüren önceki aramalardan resimleri çevrimiçi olarak bulmanızı sağlar `ImageInsightsToken` . Bu uygulama bir alır `ImageInsightsToken` ve sonraki aramada belirtecini kullanır. Daha sonra bunu `ImageInsightsToken` Bing 'e gönderir ve Bing arama URL 'leri ve çevrimiçi bulunan benzer görüntülerin URL 'lerini içeren sonuçları döndürür.

Bu öğreticinin tam kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInsightsTokens.cs)üzerinde ek hata işleme ve ek açıklamalarla bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Herhangi bir [Visual Studio 2019](https://www.visualstudio.com/downloads/)sürümü.
* Linux/MacOS kullanıyorsanız bu uygulamayı [mono](https://www.mono-project.com/)kullanarak çalıştırabilirsiniz.
* NuGet Görsel Arama ve Resim Arama paketleri.
    - Visual Studio 'daki Çözüm Gezgini projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet** ' i seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch`Paketini ve `Microsoft.Azure.CognitiveServices.Search.ImageSearch` paketini yükler. NuGet paketlerini yüklemek aşağıdakileri de yapar:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-client-library"></a>Bing Resim Arama istemci kitaplığından ımageınsibir sToken alın

Bu uygulama `ImageInsightsToken` , [Bing resim arama istemci kitaplığı](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart)aracılığıyla elde edilen bir kullanır. Yeni bir C# konsol uygulamasında, kullanarak API 'yi çağırmak için bir istemci oluşturun `ImageSearchClient()` . Sonra `SearchAsync()` Sorgunuzla birlikte kullanın:

```csharp
var client = new ImageSearchClient(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

' Yi kullanarak ilk arama sonucunu depolayın `imageResults.Value.First()` ve ardından görüntü öngörülerini saklayın `ImageInsightsToken` .

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

`ImageInsightsToken` `ImageInfo` Bing Görsel Arama yanıtlarındaki yanıtlardan bulunan öğesinden bir nesne oluşturarak görsel arama isteği için belirtin `ImageInsightsToken` .

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Imageınsilım sToken görüntülerini bulmak için Bing Görsel Arama kullanma

`VisualSearchRequest`Nesnesi, aranacağı içindeki görüntüyle ilgili bilgiler içerir `ImageInfo` . `VisualSearchMethodAsync()` yöntemi sonuçları alır. Görüntü, belirteç tarafından temsil edildiği için bir görüntü ikilisi sağlamanız gerekmez.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Görsel Arama sonuçları boyunca yineleme

Görsel Arama sonuçları `ImageTag` nesneleridir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her biri `ImageAction` `Data` , eylem türüne bağlı değerlerin listesi olan bir alan içerir. `ImageTag`İçindeki nesneler üzerinde yineleyebilirsiniz `visualSearchResults.Tags` ve `ImageAction` içindeki etiketi alabilirsiniz. Aşağıdaki örnekte eylemlerin ayrıntıları yazdırılır `PagesIncluding` :

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

Eylem türlerinden gerçek görüntü URL 'Lerinin alınması `ActionType` `ImageModuleAction` , bir değer listesi içeren bir öğe içeren bir olarak okuyan bir tür dönüştürme gerektirir `Data` . Her değer, bir görüntünün URL’sidir.  Aşağıdaki `PagesIncluding` eylem türünü öğesine yayınlar `ImageModuleAction` ve değerleri okur:

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

|EylemTürü  |URL  |
|---------|---------|
|Moyeniden boyutlanıyor-> WebSearchUrl     |         |
|VisualSearch-> WebSearchUrl     |         |
|Imagebyıd-> WebSearchUrl    |         |
|Relatedaramalarını-> WebSearchUrl:    |         |
|Documentlevelönerilere-> WebSearchUrl:     |         |
|TopicResults-> WebSearchUrl    | https: \/ /www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID = 1BA795A21EAF6A63175699B71FC36B7C&RD = 1&h = BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v = 1&r = https %3 a %2 f %2 f www. Bing. com% 2fdiscover% 2fkanada %2 brocky&p = DevEx, 5823.1       |
|Imageresults-> WebSearchUrl    |  https: \/ /www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID = 1BA795A21EAF6A63175699B71FC36B7C&RD = 1&h = PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v = 1&r = https %3 a %2 f %2 f www. Bing. com% 2fimages% 2fsearch% 3Fq% 3doutdoor&p = DevEx, 5831.1       |

Yukarıda gösterildiği gibi, `TopicResults` ve `ImageResults` türleri ilgili görüntüler için sorgular içerir. URL 'Ler Bing arama sonuçlarına bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
