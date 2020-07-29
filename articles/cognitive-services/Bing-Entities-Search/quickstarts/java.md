---
title: 'Hızlı başlangıç: Java ile REST API bir arama isteği gönderme Bing Varlık Arama'
titleSuffix: Azure Cognitive Services
description: Java kullanarak Bing Varlık Arama REST API isteği göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.custom: devx-track-java
ms.author: aahi
ms.openlocfilehash: 3afc19e93e400894a66e659d79d834b8a83132a2
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319295"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-java"></a>Hızlı başlangıç: Java kullanarak Bing Varlık Arama REST API arama isteği gönderme

Bing Varlık Arama API'si ilk çağrısını yapmak ve JSON yanıtını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit Java uygulaması, API 'ye bir haber arama sorgusu gönderir ve yanıtı görüntüler.

Bu uygulama Java 'da yazılsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/).
* [Gson kitaplığı](https://github.com/google/gson).


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

   ```java
   import java.io.*;
   import java.net.*;
   import java.util.*;
   import javax.net.ssl.HttpsURLConnection;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   ```

2. Yeni bir sınıfta, API uç noktası, abonelik anahtarınız ve bir arama sorgusu için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

   ```java
   public class EntitySearch {

      static String subscriptionKey = "ENTER KEY HERE";
    
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bing/v7.0/entities";
    
        static String mkt = "en-US";
        static String query = "italian restaurant near me";
   //...
    
   ```

## <a name="construct-a-search-request-string"></a>Arama isteği dizesi oluşturun

1. JSON döndüren adlı bir işlev oluşturun `search()` `String` . URL-arama sorgunuzu kodlayın ve ile bir parametre dizesine ekleyin `&q=` . Pazar alanınızı ile parametre dizesine ekleyin `?mkt=` .
 
2. Ana bilgisayarınız, yollarınız ve parametre Dizeleriniz ile bir URL nesnesi oluşturun.
    
    ```java
    //...
    public static String search () throws Exception {
        String encoded_query = URLEncoder.encode (query, "UTF-8");
        String params = "?mkt=" + mkt + "&q=" + encoded_query;
        URL url = new URL (host + path + params);
    //...
    ```
      
## <a name="send-a-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt alma

1. `search()`Yukarıda oluşturulan işlevde, ile yeni bir nesne oluşturun `HttpsURLConnection` `url.openCOnnection()` . İstek yöntemini olarak ayarlayın `GET` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

    ```java
    //...
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    connection.setRequestMethod("GET");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    connection.setDoOutput(true);
    //...
    ```

2. Yeni bir oluştur `StringBuilder` . `InputStreamReader`API yanıtını okumak için örneği oluşturulurken yeni bir parametre olarak kullanın `BufferedReader` .  
    
    ```java
    //...
    StringBuilder response = new StringBuilder ();
    BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
    //...
    ```

3. `String`Kaynağından yanıtı depolamak için bir nesne oluşturun `BufferedReader` . Üzerinde yineleme yapın ve her satırı dizeye ekleyin. Ardından, okuyucuyu kapatıp yanıtı döndürün. 
    
    ```java
    String line;
    
    while ((line = in.readLine()) != null) {
      response.append(line);
    }
    in.close();
    
    return response.toString();
    ```

## <a name="format-the-json-response"></a>JSON yanıtını biçimlendirme

1. JSON yanıtını biçimlendirmek için çağrılan yeni bir işlev oluşturun `prettify` . `JsonParser`JSON metninde yeni bir çağrı oluşturun, çağırın `parse()` ve bir JSON nesnesi olarak depolayın. 

2. Yeni bir oluşturmak için Gson kitaplığı kullanın, `GsonBuilder()` JSON 'u `setPrettyPrinting().create()` biçimlendirmek için kullanın ve ardından döndürün.    
  
   ```java
   //...
   public static String prettify (String json_text) {
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(json_text).getAsJsonObject();
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
   }
   //...
   ```

## <a name="call-the-search-function"></a>Search işlevini çağırın

- Projenizin Main yönteminden, çağırın `search()` ve `prettify()` Metni biçimlendirmek için kullanın.
    
    ```java
        public static void main(String[] args) {
            try {
                String response = search ();
                System.out.println (prettify (response));
            }
            catch (Exception e) {
                System.out.println (e);
            }
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

* [Bing Varlık Arama API'si nedir?](../overview.md)
* [Bing varlık arama API'si Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
