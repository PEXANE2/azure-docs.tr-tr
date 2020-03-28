---
title: "Quickstart: REST API ve Java'yı kullanarak görüntü öngörüleri alın - Bing Görsel Arama"
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine nasıl görüntü yükleyip bu konuda bilgi edineceklerini öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: fe323fc27062ad1bee9abdfaf3408430e28523a9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446626"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Quickstart: Bing Visual Search REST API ve Java'yı kullanarak görüntü öngörüleri edinin

Bing Görsel Arama API'sine ilk aramanızı yapmak ve sonuçları görüntülemek için bu hızlı başlangıcı kullanın. Bu Java uygulaması API'ye bir resim yükler ve döndürdgösterdiği bilgileri görüntüler. Bu uygulama Java'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirme Kiti (JDK) 7 veya 8](https://aka.ms/azure-jdks)
* [Gson Java kütüphanesi](https://github.com/google/gson)
* [Apache Bileşenleri](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Favori IDE veya düzenleyicinizde yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları aktarın:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. API bitiş noktanız, abonelik anahtarınız ve resminize giden yol için değişkenler oluşturun. `endpoint`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir:

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

    
    Yerel bir resim yüklediğinizde, form verilerinin üstbilgiiç `Content-Disposition` içermesi gerekir. Parametresini `name` "görüntü" olarak ayarlamanız gerekir ve `filename` parametreyi herhangi bir dize ayarlayabilirsiniz. Formun içeriği görüntünün ikili verilerini içerir. Yükleyebileceğiniz maksimum görüntü boyutu 1 MB'dır.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

## <a name="create-the-json-parser"></a>JSON parer'ını oluşturun

API'den Gelen JSON yanıtını kullanarak `JsonParser`daha okunabilir hale getirmek için bir yöntem oluşturun:

```java
public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturma

1. Uygulamanızın ana yönteminde, aşağıdakileri kullanarak `HttpClientBuilder.create().build();`bir HTTP istemcisi oluşturun:

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Resminizi `HttpEntity` API'ye yüklemek için bir nesne oluşturun:

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Bitiş `httpPost` noktanızla bir nesne oluşturun ve üstbilginin abonelik anahtarınızı kullanmasını ayarlayın:

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>JSON yanıtını alma ve işleme

1. Bir `HttpClient.execute()` istek API'ye göndermek ve yanıtı bir `InputStream` nesnede depolamak için yöntemi kullanın:
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. JSON dizesini depolayın ve yanıtı yazdırın:

    ```java
    String json = new Scanner(stream).useDelimiter("\\A").next();
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(json));
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalık web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
