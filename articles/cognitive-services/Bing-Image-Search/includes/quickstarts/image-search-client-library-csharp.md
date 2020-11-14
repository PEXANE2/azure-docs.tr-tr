---
title: Bing Resim Arama C# istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/21/2020
ms.author: aahi
ms.openlocfilehash: 247aea49d60c2d953d8b6dff37d22188a5442a1f
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625193"
---
Bing Resim Arama istemci kitaplığını kullanarak ilk görüntünüzü aramanızı sağlamak için bu hızlı başlangıcı kullanın. 

İstemci arama kitaplığı, REST API için bir sarmalayıcıdır ve aynı özellikleri içerir. 

Bir C# uygulaması oluşturacak bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL 'sini görüntüler.

Bu örneğe ilişkin kaynak kodu, ek hata işleme ve ek açıklama ile [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* Windows kullanıyorsanız, herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/vs/whatsnew/) sürümü
* MacOS veya Linux kullanıyorsanız, [.NET Core yüklü](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install) [vs Code](https://code.visualstudio.com)
* [Ücretsiz bir Azure aboneliği](https://azure.microsoft.com/free/dotnet)

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

Ayrıca bkz. bilişsel [Hizmetler fiyatlandırması-BING arama API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-a-console-project"></a>Konsol projesi oluşturma

İlk olarak, yeni bir C# konsol uygulaması oluşturun.

## <a name="create-a-console-project"></a>Konsol projesi oluşturma

# <a name="visual-studio"></a>[Visual Studio](#tab/visualstudio)

1. Visual Studio 'da *Bingimagesearch* adlı yeni bir konsol çözümü oluşturun.
    
1. Bilişsel [resim arama NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch) ekleme
    1. **Çözüm Gezgini** ' de projenize sağ tıklayın.
    1. **NuGet Paketlerini Yönet** ' i seçin.
    1. *Microsoft. Azure. Biliveservices. Search. ımagesearch* için arama yapın ve seçin ve ardından paketi yükler.
    
# <a name="vs-code"></a>[VS Code](#tab/vscode)

1. VS Code içinde Terminal penceresini açın.
1. Terminal penceresine aşağıdaki kodu girerek *Bingimagesearch* adlı yeni bir konsol projesi oluşturun:
    
    ```bash
    dotnet new console -n BingImageSearch
    ```
1. VS Code 'de *Bingimagesearch* klasörünü açın.
1. Terminal penceresine aşağıdaki kodu girerek bilişsel [resim arama NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch) nugetpackage ' i ekleyin:

    ```bash
    dotnet add package Microsoft.Azure.CognitiveServices.Search.ImageSearch
    ```

---

## <a name="initialize-the-application"></a>Uygulamayı başlatma


1. `using` *Program.cs* içindeki tüm deyimleri şu kodla değiştirin:

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
    using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
    ```

1. `Main`Projenizin yönteminde, geçerli abonelik anahtarınız için değişkenler oluşturun, görüntünün Bing tarafından döndürülmesini ve bir arama terimini elde edin. Ardından anahtarı kullanılarak görüntü arama istemcisinin bir örneğini oluşturun.

    ```csharp
    static async Task Main(string[] args)
    {
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
    }
    ```
    
## <a name="send-a-search-query-using-the-client"></a>İstemciyi kullanarak bir arama sorgusu gönderme
    
Hala `Main` yönteminde, bir sorgu metniyle arama yapmak için istemcisini kullanın:
    
```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = await client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Birinci görüntü sonucunu ayrıştırma ve görüntüleme

Yanıtta döndürülen resim sonuçlarını ayrıştırın. 

Yanıt, arama sonuçları içeriyorsa, ilk sonucu depolayın ve bazı ayrıntılarını yazdırın.

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.WriteLine("Press any key to exit ...");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](../../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](../../overview.md)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Azure Bilişsel Hizmetler SDK’sı için .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Azure Bilişsel Hizmetler Belgeleri](../../../index.yml)
* [Bing Resim Arama API’si başvurusu](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)