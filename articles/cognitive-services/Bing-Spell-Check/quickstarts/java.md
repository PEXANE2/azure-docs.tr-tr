---
title: 'Quickstart: REST API ve Java ile yazım denetimi - Bing Yazım Denetimi'
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
ms.openlocfilehash: 43c528a1e9e46a67e895679b1a0fd04fef8900a7
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75382968"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-java"></a>Quickstart: Bing Yazım Denetimi REST API ve Java ile yazım denetimi

Bing Yazım Denetimi REST API'ye ilk aramanızı yapmak için bu hızlı başlangıcı kullanın. Bu basit Java uygulaması API'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. Bu uygulama Java'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir. Bu uygulamanın kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingSpellCheckv7.java)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* Java Geliştirme Kiti(JDK) 7 veya daha sonra.

* [Gson-2.8.5.jar](https://libraries.io/maven/com.google.code.gson%3Agson) veya en güncel [Gson](https://github.com/google/gson) sürümünü ithal edin. Komut satırı yürütme için, ana sınıf ile Java klasörünüze ekleyin. `.jar`

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Bir uygulama oluşturma ve başlatma

1. Favori IDE'nizde veya seçtiğiniz bir sınıf adı olan bir editörde yeni bir Java Projesi oluşturun ve ardından aşağıdaki paketleri aktarın.

    ```java
    import java.io.*;
    import java.net.*;
    import com.google.gson.*;
    import javax.net.ssl.HttpsURLConnection;
    ```

2. API bitiş noktasının ana bilgisayarı, yolu ve abonelik anahtarınız için değişkenler oluşturun. Ardından, pazarınız için değişkenler, denetimi hecelemek istediğiniz metin ve yazım denetimi modu için bir dize oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```java
    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    static String key = "<ENTER-KEY-HERE>";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";
    ```

## <a name="create-and-send-an-api-request"></a>API isteği oluşturma ve gönderme

1. API isteğini `check()` oluşturmak ve göndermek için çağrılan bir işlev oluşturun. İçinde, aşağıdaki adımları izleyin. İstek parametreleri için bir dize oluşturun. parametreyi `?mkt=` pazar dizenize, parametreyi `&mode=` de yazım denetimi moduna ekleyin.  

   ```java
   public static void check () throws Exception {
       String params = "?mkt=" + mkt + "&mode=" + mode;
      // add the rest of the code snippets here (except prettify() and main())...
   }
   ```

2. Bitiş noktası ana bilgisayarını, yolu ve parametreler dizesini birleştirerek bir URL oluşturun. Yeni `HttpsURLConnection` bir nesne oluşturun.

    ```java
    URL url = new URL(host + path + params);
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    ```

3. URL'ye bağlantı açın. İstek yöntemini `POST`' ' ye ayarlayın İstek parametrelerinizi ekleyin. Abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` eklediğinizden emin olun.

    ```java
    connection.setRequestMethod("POST");
    connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
    connection.setDoOutput(true);
    ```

4. Yeni `DataOutputStream` bir nesne oluşturun ve isteği API'ye gönderin.

    ```java
        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();
    ```

## <a name="format-and-read-the-api-response"></a>API yanıtını biçimlendirin ve okuyun

1. Bu yöntemi sınıfınıza ekleyin. Daha okunabilir bir çıkış için JSON biçimlendirin.

    ``` java
    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    ```

1. Bir `BufferedReader` oluştur ve API'den yanıtı okuyun. Konsola yazdırın.
    
    ```java
    BufferedReader in = new BufferedReader(
    new InputStreamReader(connection.getInputStream()));
    String line;
    while ((line = in.readLine()) != null) {
        System.out.println(prettify(line));
    }
    in.close();
    ```

## <a name="call-the-api"></a>API çağırma

Uygulamanızın ana işlevinde, yukarıda oluşturulan çek() yöntemini arayın.
```java
        public static void main(String[] args) {
            try {
                check();
            }
            catch (Exception e) {
                System.out.println (e);
            }
        }
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi oluşturun ve çalıştırın.

Komut satırını kullanıyorsanız, uygulamayı oluşturmak ve çalıştırmak için aşağıdaki komutları kullanın.

**Oluşturmak:**
```bash
javac -classpath .;gson-2.2.2.jar\* <CLASS_NAME>.java
```

**Çalıştırın:**
```bash
java -cp .;gson-2.2.2.jar\* <CLASS_NAME>
```

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
