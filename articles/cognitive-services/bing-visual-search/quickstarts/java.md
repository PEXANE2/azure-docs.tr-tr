---
title: 'Hızlı başlangıç: REST API ve Java-Bing Görsel Arama kullanarak görüntü öngörülerini alın'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si bir görüntüyü karşıya yüklemeyi ve ilgili öngörüleri nasıl alabileceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.custom: devx-track-java
ms.author: scottwhi
ms.openlocfilehash: 6e0e0cc2513b1c2a5f89e61984331399ebae269a
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320451"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Java kullanarak görüntü öngörülerini alın

Bing Görsel Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu Java uygulaması, API 'ye bir görüntü yükler ve döndürdüğü bilgileri görüntüler. Bu uygulama Java 'da yazılsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Java Development Kit (JDK) 7 veya 8](https://aka.ms/azure-jdks)
* [Gson Java kitaplığı](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

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

2. API uç noktanız, abonelik anahtarınız ve görüntünüzün yolu için değişkenler oluşturun. Değer için `endpoint` aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure Portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

    
3. Yerel bir görüntüyü karşıya yüklediğinizde, form verileri `Content-Disposition` üstbilgiyi içermelidir. `name`Parametresini "image" olarak ayarlayın ve bu `filename` parametreyi görüntünün dosya adı olarak ayarlayın. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz en büyük görüntü boyutu 1 MB 'tır.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

## <a name="create-the-json-parser"></a>JSON ayrıştırıcısı oluşturma

Kullanarak, API 'den gelen JSON yanıtının daha okunaklı olmasını sağlamak için bir yöntem oluşturun `JsonParser` .

```java
public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturma

1. Uygulamanızın ana yönteminde, kullanarak bir HTTP istemcisi oluşturun `HttpClientBuilder.create().build();` .

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. `HttpEntity`GÖRÜNTÜNÜZÜ API 'ye yüklemek için bir nesne oluşturun.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Uç noktanızla bir `httpPost` nesne oluşturun ve üst bilgiyi abonelik anahtarınızı kullanacak şekilde ayarlayın.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>JSON yanıtını alma ve işleme

1. `HttpClient.execute()`API 'ye bir istek göndermek ve yanıtı bir nesne içinde depolamak için yöntemini kullanın `InputStream` .
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. JSON dizesini depolayın ve yanıtı yazdırın.

    ```java
    String json = new Scanner(stream).useDelimiter("\\A").next();
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(json));
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
