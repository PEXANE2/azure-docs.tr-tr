---
title: Imageınsibustoken-Bing Görsel Arama kullanarak önceki aramalardan benzer görüntüleri bulma
titleSuffix: Azure Cognitive Services
description: Imageınsi, Stoken tarafından belirtilen görüntülerin URL 'Lerini almak için Bing Görsel Arama SDK 'sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 11/29/2019
ms.author: aahi
ms.openlocfilehash: dff96b19f40c2d897b6a018a4c46cec60f8aa201
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689316"
---
# <a name="find-similar-images-from-previous-searches-using-imageinsightstoken"></a>Imageınsilım sToken kullanarak önceki aramalardan benzer görüntüleri bulma

Görsel Arama SDK, bir `ImageInsightsToken`döndüren önceki aramalardan çevrimiçi olarak görüntü bulmanıza olanak sağlar. Bu uygulama bir `ImageInsightsToken` alır ve sonraki aramada belirtecini kullanır. Daha sonra `ImageInsightsToken` Bing 'e gönderir ve çevrimiçi bulunan benzer görüntülerin Bing Arama URL 'lerini ve URL 'Leri içeren sonuçları döndürür.

Bu öğreticinin tam kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInsightsTokens.cs)üzerinde ek hata işleme ve ek açıklamalarla bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir [Visual Studio 2019](https://www.visualstudio.com/downloads/)sürümü.
* Linux/MacOS kullanıyorsanız bu uygulamayı [mono](https://www.mono-project.com/)kullanarak çalıştırabilirsiniz.
* NuGet Görsel Arama ve Resim Arama paketleri.
    - Visual Studio 'daki Çözüm Gezgini projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet** ' i seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini ve `Microsoft.Azure.CognitiveServices.Search.ImageSearch` paketini yükler. NuGet paketlerini yüklemek aşağıdakileri de yapar:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-sdk"></a>Bing Resim Arama SDK 'dan ımageınsibir sToken alın

Bu uygulama [BING resım arama SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart)aracılığıyla elde edilen bir `ImageInsightsToken` kullanır. Yeni C# bir konsol uygulamasında, `ImageSearchClient()`kullanarak API 'yi çağırmak için bir istemci oluşturun. Daha sonra Sorgunuzla `SearchAsync()` kullanın:

```csharp
var client = new ImageSearchClient(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

`imageResults.Value.First()`kullanarak ilk arama sonucunu depolayın ve ardından görüntünün Insight `ImageInsightsToken`saklayın.

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

Bu `ImageInsightsToken` bir istekte Bing Görsel Arama gönderilir.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>Görsel Arama isteğine ımageınsibir sToken ekleyin

Bing Görsel Arama yanıtlarındaki `ImageInsightsToken` `ImageInfo` nesne oluşturarak bir Görsel Arama isteği için `ImageInsightsToken` belirtin.

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Imageınsilım sToken görüntülerini bulmak için Bing Görsel Arama kullanma

`VisualSearchRequest` nesnesi, aranacak `ImageInfo` içindeki görüntüyle ilgili bilgiler içerir. `VisualSearchMethodAsync()` yöntemi sonuçları alır. Görüntü, belirteç tarafından temsil edildiği için bir görüntü ikilisi sağlamanız gerekmez.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Görsel Arama sonuçları boyunca yineleme

Görsel Arama sonuçları `ImageTag` nesneleridir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her `ImageAction`, eylem türüne bağlı değerlerin listesi olan bir `Data` alanı içerir. Örneğin, `visualSearchResults.Tags``ImageTag` nesneleri boyunca yineleyebilirsiniz ve içindeki `ImageAction` etiketini bulabilirsiniz. Aşağıdaki örnekte `PagesIncluding` eylemlerin ayrıntıları yazdırılır:

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

Eylem türlerinden gerçek görüntü URL 'Lerinin alınması, bir `ActionType` `ImageModuleAction`olarak okuyan bir tür `Data` öğesi içeren bir atama gerektirir. Her değer, bir görüntünün URL’sidir.  Aşağıdaki `PagesIncluding` eylem türünü `ImageModuleAction` ve değerleri okur:

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

|actionType  |URL  | |
|---------|---------|---------|
|Moyeniden boyutlanıyor-> WebSearchUrl     |         |
|VisualSearch-> WebSearchUrl     |         |
|Imagebyıd-> WebSearchUrl    |         |
|Relatedaramalarını-> WebSearchUrl:    |         |
|Documentlevelönerilere-> WebSearchUrl:     |         |
|TopicResults-> WebSearchUrl    | https:\//www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1       |
|Imageresults-> WebSearchUrl    |  https:\//www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1       |

Yukarıda gösterildiği gibi, `TopicResults` ve `ImageResults` türleri ilgili görüntüler için sorgular içerir. URL 'Ler Bing arama sonuçlarına bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
