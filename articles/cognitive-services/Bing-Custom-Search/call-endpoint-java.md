---
title: 'Hızlı başlangıç: Java kullanarak Bing Özel Arama uç noktanızı çağırma | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Java 'daki Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: 7c9f713d50e31557999c9127cd219bfe9b3a8c82
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83199849"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-java"></a>Hızlı başlangıç: Java kullanarak Bing Özel Arama uç noktanızı çağırma

Bing Özel Arama örneğinden arama sonuçları isteme hakkında bilgi edinmek için bu hızlı başlangıcı kullanın. Bu uygulama Java 'da yazılsa da, Bing Özel Arama API'si çoğu programlama dili ile uyumlu olan bir yeniden yazılmış Web hizmetidir. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingCustomSearchv7.java)' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](quick-start.md).

- En son [Java geliştirme seti](https://www.oracle.com/technetwork/java/javase/downloads/index.html).

- [Gson kitaplığı](https://github.com/google/gson).

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

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

2. Adlı bir sınıf oluşturun `CustomSrchJava` ve ardından abonelik anahtarınız, özel arama uç noktası ve arama örneğinizin özel yapılandırma kimliği için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.
    ```java
    public class CustomSrchJava {
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";
        static String searchTerm = "Microsoft";  
    ...
    ```

3. `SearchResults`Bing özel arama örneğinden gelen yanıtı içermesi için adlı başka bir sınıf oluşturun.

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

4. `prettify()`BING özel arama API'SI JSON yanıtını biçimlendirmek için adlı bir işlev oluşturun.

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

1. `SearchWeb()`Bir istek gönderen ve bir nesne döndüren adlı bir işlev oluşturun `SearchResults` . Özel yapılandırma KIMLIĞINIZI, sorgunuzu ve uç nokta bilgilerinizi birleştirerek istek URL 'sini oluşturun. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

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

2. Bir akış oluşturun ve JSON yanıtını bir `SearchResults` nesneye depolayın.

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

3. JSON yanıtını yazdır.

    ```java
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    ```

4. Programı çalıştırın.
    
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir arama Web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
