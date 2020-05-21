---
title: 'Hızlı başlangıç: C# kullanarak REST API bir arama isteği gönderme Bing Varlık Arama'
titleSuffix: Azure Cognitive Services
description: C# kullanarak Bing Varlık Arama REST API isteği göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: cd89f2ae13b10c83c3fc22023fc2e3cae1770c98
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650282"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-c"></a>Hızlı başlangıç: C kullanarak Bing Varlık Arama REST API arama isteği gönderme #

Bing Varlık Arama API'si ilk çağrısını yapmak ve JSON yanıtını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit C# uygulaması, API 'ye bir haber arama sorgusu gönderir ve yanıtı görüntüler. Bu uygulamanın kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingEntitySearchv7.cs)' da kullanılabilir.

Bu uygulama C# dilinde yazılsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.


## <a name="prerequisites"></a>Ön koşullar

- Herhangi bir [Visual Studio 2017 veya üzeri](https://www.visualstudio.com/downloads/)sürümü.

- NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi. NuGet paketini Visual Studio 'ya yüklemek için:

   1. **Çözüm Gezgini**' de projenize sağ tıklayın.
   2. **NuGet Paketlerini Yönet**' i seçin.
   3. *Newtonsoft. JSON*öğesini arayıp seçin ve ardından paketi yükler.

- Linux/MacOS kullanıyorsanız, bu uygulama [mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio 'da yeni bir C# konsol çözümü oluşturun. Ardından, aşağıdaki ad alanlarını ana kod dosyasına ekleyin:
    
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    ```

2. Yeni bir sınıf oluşturun ve API uç noktası, abonelik anahtarınız ve aramak istediğiniz sorgu için değişkenler ekleyin. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```csharp
    namespace EntitySearchSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/entities";
    
            static string market = "en-US";
    
            // NOTE: Replace this example key with a valid subscription key.
            static string key = "ENTER YOUR KEY HERE";
    
            static string query = "italian restaurant near me";
        //...
        }
    }
    ```

## <a name="send-a-request-and-get-the-api-response"></a>İstek Gönder ve API yanıtını al

1. Sınıfı içinde adlı bir işlev oluşturun `Search()` . Bu işlev içinde yeni bir nesne oluşturun `HttpClient` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

2. Konağı ve yolu birleştirerek isteğiniz için URI 'yi oluşturun. Ardından, Pazar ve URL 'nizi ekleyin-sorgunuzu kodlayın.

3. `client.GetAsync()`Http yanıtı almak için Await ve ardından bekleyen JSON yanıtını saklayın `ReadAsStringAsync()` .

4. JSON dizesini ile biçimlendirin `JsonConvert.DeserializeObject()` ve konsola yazdırın.

      ```csharp
      async static void Search()
      {
       //...
       HttpClient client = new HttpClient();
       client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

       string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

       HttpResponseMessage response = await client.GetAsync(uri);

       string contentString = await response.Content.ReadAsStringAsync();
       dynamic parsedJson = JsonConvert.DeserializeObject(contentString);
       Console.WriteLine(parsedJson);
      }
      ```

5. `Main()`Uygulamanızın yönteminde, `Search()` işlevini çağırın.
    
    ```csharp
    static void Main(string[] args)
    {
        Search();
        Console.ReadLine();
    }
    ```


## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../overview.md )
* [Bing varlık arama API'si Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
