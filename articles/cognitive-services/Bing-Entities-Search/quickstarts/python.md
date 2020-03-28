---
title: "Hızlı başlangıç: Python kullanarak REST API'ye arama isteği gönderme - Bing Entity Search"
titleSuffix: Azure Cognitive Services
description: Python'u kullanarak Bing Entity Search REST API'sine istek göndermek ve JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 12/11/2019
ms.author: aahi
ms.openlocfilehash: 07b563308e80055d699d1cefeb3b2db71ffa4cd7
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448610"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-python"></a>Hızlı başlatma: Python kullanarak Bing Entity Search REST API'sine arama isteği gönderme

Bing Entity Search API'ye ilk aramanızı yapmak ve JSON yanıtını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması API'ye bir haber arama sorgusu gönderir ve yanıtı görüntüler. Bu örnek için kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py)kullanılabilir.

Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [Piton](https://www.python.org/downloads/) 2.x veya 3.x

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri almaları ekleyin. Abonelik anahtarınız, bitiş noktanız, pazarınız ve arama sorgunuz için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```python
    import http.client, urllib.parse
    import json
    
    subscriptionKey = 'ENTER YOUR KEY HERE'
    host = 'api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

2. Piyasa değişkeninizi parametreye ekleyerek bir `?mkt=` istek url'si oluşturun. Sorgunuzu url kodlayın ve parametreye ekleyin. `&q=` 
    
    ```python
    params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)
    ```

## <a name="send-a-request-and-get-a-response"></a>İstek gönderme ve yanıt alma

1. 'li bir `get_suggestions()`işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.
   1. Abonelik anahtarınızı anahtar olarak `Ocp-Apim-Subscription-Key` bir sözlük olarak bir sözlük ekleyin.
   2. BIR `http.client.HTTPSConnection()` HTTPS istemci nesnesi oluşturmak için kullanın. Yolunuzu `GET` ve `request()` parametrelerinizi ve üstbilgi bilgilerinizi kullanarak bir istek gönderin.
   3. Yanıtı `getresponse()`, ile depolayın ve döndürün. `response.read()`

      ```python
      def get_suggestions ():
       headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
       conn = http.client.HTTPSConnection (host)
       conn.request ("GET", path + params, None, headers)
       response = conn.getresponse ()
       return response.read()
      ```

2. Arama `get_suggestions()`ve json yanıtını yazdırın.

    ```python
    result = get_suggestions ()
    print (json.dumps(json.loads(result), indent=4))
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
> [Tek sayfalık bir web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Entity Arama API nedir](../search-the-web.md)
* [Bing Varlık Arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
