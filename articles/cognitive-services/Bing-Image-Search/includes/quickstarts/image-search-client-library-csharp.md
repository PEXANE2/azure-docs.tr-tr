---
title: Bing Resim Arama C# istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: 9e74742858c5de2abf56b80ea4e4f275bc2c9b23
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899576"
---
API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Resim Arama istemci kitaplığını kullanarak ilk görüntünüzü aramanızı sağlamak için bu hızlı başlangıcı kullanın. Bu basit C# uygulaması bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL’sini görüntüler.

Bu örneğin kaynak kodu, ek hata işleme ve açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch)’da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar
* Herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/vs/whatsnew/)sürümü.
* [Bilişsel Görüntü Arama NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/).

Visual Studio 'da Bing Resim Arama istemci kitaplığını yüklemek için, **Çözüm Gezgini**' dan **NuGet Paketlerini Yönet** seçeneğini kullanın.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

Ayrıca bkz. bilişsel [Hizmetler fiyatlandırması-BING arama API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

İlk olarak Visual Studio’da yeni bir C# konsol uygulaması oluşturun. Ardından projenize aşağıdaki paketleri ekleyin.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

Projenizin main yönteminde geçerli abonelik anahtarınız, Bing tarafından döndürülecek görüntü sonuçları ve bir arama terimi için değişkenler oluşturun. Ardından anahtarı kullanılarak görüntü arama istemcisinin bir örneğini oluşturun.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";

//initialize the client
//NOTE: If you're using version 1.2.0 or below for the Bing Image Search client library, 
// use ImageSearchAPI() instead of ImageSearchClient() to initialize your search client.

var client = new ImageSearchClient(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>İstemciyi kullanarak bir arama sorgusu gönderme

Bir sorgu metniyle arama yapmak için istemciyi kullanın:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Birinci görüntü sonucunu ayrıştırma ve görüntüleme

Yanıtta döndürülen resim sonuçlarını ayrıştırın.
Yanıt arama sonuçları içeriyorsa, ilk sonucu depolayın ve döndürülen toplam görüntü sayısının yanı sıra bu ilk sonucun küçük resim URL'si, asıl URL gibi ayrıntılarını yazdırın.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler SDK’sı için .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
