---
title: 'Quickstart: REST API ve C# ile yazım denetimi - Bing Yazım Denetimi'
titleSuffix: Azure Cognitive Services
description: Yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API'sini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: aahi
ms.openlocfilehash: 036ea00362b604957a1887127fca0b8d775d4e7b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75382974"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-c"></a>Quickstart: Bing Yazım Denetimi REST API ve C ile yazım denetimi #

Bing Yazım Denetimi REST API'ye ilk aramanızı yapmak için bu hızlı başlangıcı kullanın. Bu basit C# uygulaması API'ye bir istek gönderir ve önerilen düzeltmelerin listesini döndürür. Bu uygulama C# ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingAutosuggestv7.cs)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017 veya sonrası](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* Visual `Newtonsoft.Json` studio'da NuGet paketi olarak yüklemek için:
    1. **Çözüm Gezgini'nde,** Çözüm dosyasına sağ tıklayın.
    1. **Çözüm için NuGet Paketlerini Yönet'i**seçin.
    1. Paketi `Newtonsoft.Json` arayın ve yükleyin.
* Linux/MacOS kullanıyorsanız, bu uygulama [Mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio'da `SpellCheckSample` yeni bir konsol çözümü oluşturun. Ardından ana kod dosyasına aşağıdaki ad alanlarını ekleyin.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Newtonsoft.Json;
    ```

2. API bitiş noktası, abonelik anahtarınız ve yazım denetimi yapılacak metin için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```csharp
    namespace SpellCheckSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/spellcheck?";
            static string key = "<ENTER-KEY-HERE>";
            //text to be spell-checked
            static string text = "Hollo, wrld!";
        }
    }
    ```

3. Arama parametreleriniz için bir değişken oluşturun. Sonra pazar kodunuzu `mkt=`ekle. Piyasa kodu, talepte bulunduğunuz ülkedir. Ayrıca, yazım denetimi modunuzu `&mode=`sonra ekleyin. Mod ya `proof` (çoğu yazım/dilbilgisi hatalarını yakalar) ya da `spell` (çoğu yazım ı yakalar, ancak o kadar çok dilbilgisi hatası yakalamaz).
    
    ```csharp
    static string params_ = "mkt=en-US&mode=proof";
    ```

## <a name="create-and-send-a-spell-check-request"></a>Yazım denetimi isteği oluşturma ve gönderme

1. API'ye istek göndermek `SpellCheck()` için çağrılan bir eşzamanlı işlev oluşturun. Bir `HttpClient`, ' oluşturun ve `Ocp-Apim-Subscription-Key` abonelik anahtarınızı üstbilgiye ekleyin. Ardından işlev içinde aşağıdaki adımları gerçekleştirin.

    ```csharp
    async static void SpellCheck()
    {
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

        HttpResponseMessage response = null;
        // add the rest of the code snippets here (except for main())...
    }
    ```

2. Ev sahibinize, yola ve parametrelere ekleyerek isteğiniz için URI'yi oluşturun.
    
    ```csharp
    string uri = host + path + params_;
    ```

3. Metninizi içeren `KeyValuePair` bir nesneyle bir liste oluşturun `FormUrlEncodedContent` ve bir nesne oluşturmak için kullanın. Üstbilgi bilgilerini ayarlayın ve `PostAsync()` isteği göndermek için kullanın.

    ```csharp
    var values = new Dictionary<string, string>();
    values.Add("text", text);
    var content = new FormUrlEncodedContent(values);
    content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded");
    response = await client.PostAsync(uri, new FormUrlEncodedContent(values));
    ```

## <a name="get-and-print-the-api-response"></a>API yanıtını alın ve yazdırın

### <a name="get-the-client-id-header"></a>İstemci kimliği üstbilgisini alma

Yanıt bir `X-MSEdge-ClientID` üstbilgi içeriyorsa, değeri alın ve yazdırın.

``` csharp
string client_id;
if (response.Headers.TryGetValues("X-MSEdge-ClientID", out IEnumerable<string> header_values))
{
    client_id = header_values.First();
    Console.WriteLine("Client ID: " + client_id);
}
```

### <a name="get-the-response"></a>Yanıtı alın

API'den yanıt alın. JSON nesnesini deserialize edin ve konsola yazdırın.

```csharp
string contentString = await response.Content.ReadAsStringAsync();

dynamic jsonObj = JsonConvert.DeserializeObject(contentString);
Console.WriteLine(jsonObj);
```

## <a name="call-the-spell-check-function"></a>Yazım denetimi işlevini arama

Projenizin Ana işlevinde. `SpellCheck()`

```csharp
static void Main(string[] args)
{
    SpellCheck();
    Console.ReadLine();
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi oluşturun ve çalıştırın. Visual Studio kullanıyorsanız, dosyayı hata ayıklamak için **F5** tuşuna basın.

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing Yazım Denetimi API’si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
