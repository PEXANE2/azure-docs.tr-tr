---
title: 'Öğretici: Bing Görsel Arama SDK ile bir görüntü kırpma'
description: Görüntüdeki belirli ares'lerden öngörüler almak için Bing Görsel Arama SDK'sını kullanın.
services: cognitive-services
titleSuffix: Azure Cognitive Services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/31/2019
ms.author: aahi
ms.openlocfilehash: 4778a4089c7374c1ac6a9312064dcfb1e0325b63
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478487"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Öğretici: C için Bing Görsel Arama SDK ile bir görüntü kırpma #

Bing Görsel Arama SDK benzer çevrimiçi görüntüleri bulmadan önce bir görüntü kırpmanızı sağlar. Bu uygulama, birden fazla kişi içeren bir resimden tek bir kişiyi eker ve ardından çevrimiçi bulunan benzer görüntüleri içeren arama sonuçlarını döndürür.

Bu uygulama için tam kaynak kodu [github](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs)üzerinde ek hata işleme ve ek açıklamalar ile kullanılabilir.

Bu öğretici nasıl gösteriş gösterir:

> [!div class="checklist"]
> * Bing Görsel Arama SDK'sını kullanarak istek gönderme
> * Bing Görsel Arama ile arama yapmak için görüntü alanı kırpma
> * Yanıtı alma ve işleme
> * Yanıttaki eylem öğelerinin URL'lerini bulma

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
* [NuGet Özel Arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) paketinin yüklenmiş olması.
    - Visual Studio'daki Solution Explorer'dan projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet'i** seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin. NuGet Özel Arama paketini yüklediğinizde aşağıdaki derlemeler de yüklenir:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Görüntü kırpma alanını belirtin

Bu uygulama, Microsoft üst düzey liderlik ekibinin bu görüntünün bir alanını oluşturur. Bu kırpma alanı, tüm görüntünün yüzdesi olarak temsil edilen üst-sol ve alt-sağ koordinatları kullanılarak tanımlanır:  

![Microsoft Üst Düzey Liderlik Ekibi](./media/MS_SrLeaders.jpg)

Bu görüntü kırpma alanından `ImageInfo` bir nesne oluşturarak kırpılır ve nesneyi `ImageInfo` bir `VisualSearchRequest`. Nesne `ImageInfo` ayrıca resmin URL'sini de içerir:

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Kırpma alanına benzer görüntüleri arama

Değişken, `VisualSearchRequest` resmin kırpma alanı ve URL'si hakkında bilgi içerir. Yöntem `VisualSearchMethodAsync()` sonuçları alır:

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result;

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>URL verilerini`ImageModuleAction`

Bing Görsel Arama `ImageTag` sonuçları nesnelerdir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her `ImageAction` biri, eylem türüne bağlı değerlerin bir listesi olan bir `Data` alan içerir.

Aşağıdaki kodile çeşitli türleri yazdırabilirsiniz:

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

Tam uygulama şunları döndürür:

|EylemTürü  |URL'si  | |
|---------|---------|---------|
|WebSearchURL Dahil Sayfalar     |         |
|MoreSizes WebSearchURL     |         |  
|VisualSearch WebSearchURL    |         |
|ImageById WebSearchURL     |         |  
|RelatedSearchs WebSearchURL     |         |
|Varlık -> WebSearchUrl     | https\://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |
|Konu Sonuçları -> WebSearchUrl    |  https\://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |
|ImageResults -> WebSearchUrl    |  https\://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJ szgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |

Yukarıda gösterildiği `Entity` gibi, ActionType tanınabilir bir kişi, yer veya şey hakkında bilgi döndüren bir Bing arama sorgusu içerir. `TopicResults` ve `ImageResults` türleri, ilgili görüntülerin sorgularını içerir. Listedeki URL’ler Bing arama sonuçlarına yönlendirir.

## <a name="get-urls-for-pagesincluding-actiontype-images"></a>Görüntüler için `PagesIncluding` `ActionType` URL'ler alın

Gerçek görüntü URL’lerini almak için, bir `ActionType` türünü `ImageModuleAction` olarak okuyan bir tür dönüştürme gerekir. Bu tür ise değerler listesine sahip bir `Data` öğesi içerir. Her değer, bir görüntünün URL’sidir. Aşağıdaki `PagesIncluding` eylem türünü atar `ImageModuleAction` ve değerleri okur:

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalık web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.
> [Bing Görsel Arama API’si nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview)
