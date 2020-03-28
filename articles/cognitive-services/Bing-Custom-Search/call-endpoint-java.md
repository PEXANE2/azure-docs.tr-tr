---
title: 'Quickstart: Java kullanarak Bing Özel Arama bitiş noktasını arayın | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Java'daki Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 02c86e5a4c1a04b98ebba73653980e8e5e00f645
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80238886"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-java"></a>Quickstart: Java'yı kullanarak Bing Özel Arama bitiş noktanızı arayın

Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın. Bu uygulama Java'da yazılmış olsa da, Bing Özel Arama API'si çoğu programlama diliyle uyumlu bir RESTful web hizmetidir. Bu örnek için kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingCustomSearchv7.java)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

- Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](quick-start.md)

- En son [Java Geliştirme Kiti](https://www.oracle.com/technetwork/java/javase/downloads/index.html)  

- [Gson kitaplığı](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLEncoder;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Scanner;
    import javax.net.ssl.HttpsURLConnection;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    ```

2. Adlı `CustomSrchJava`bir sınıf oluşturun ve abonelik anahtarınız, özel arama bitiş noktanız ve arama örneğinizin Özel Yapılandırma Kimliği için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.
    ```java
    public class CustomSrchJava {
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";
        static String searchTerm = "Microsoft";  
    ...
    ```

3. Bing Özel `SearchResults` Arama örneğinden gelen yanıtı içerecek şekilde başka bir sınıf oluşturun.

    ```csharp
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }
    ```

4. Bing Özel `prettify()` Arama API'sinden JSON yanıtını biçimlendirmek için adlı bir işlev oluşturun.

    ```java
        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma 

1. Bir istek `SearchWeb()` gönderen ve bir nesne `SearchResults` döndüren adlandırılmış bir işlev oluşturun. Özel Yapılandırma Kimliğinizi, sorgunuzu ve bitiş noktası bilgilerinizi birleştirerek istek url'sini oluşturun. Abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` ekleyin.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            // construct the URL for your search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchTerm, "UTF-8") + "&CustomConfig=" + customConfigId);
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    ...
    ```

2. Bir akış oluşturun ve JSON `SearchResults` yanıtını bir nesnede depolayın.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            ...
            // receive the JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();
        
            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);
            
            stream.close();
            return results;
        }
    ```

3. Uygulamanızın ana yönteminde, `SearchWeb()` arama teriminizle arama, 

    ```java
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    ```

4. Programı çalıştırın.
    
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Arama web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
