---
title: 'Hızlı başlangıç: REST API ve Java-Bing Yazım Denetimi ile yazım denetimi yapma'
titleSuffix: Azure Cognitive Services
description: Yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 05/21/2020
ms.author: aahi
ms.openlocfilehash: 70ed795032c3d6d46aa57afaf5b5ece94bc74ea9
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995982"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-java"></a>Hızlı başlangıç: Bing Yazım Denetimi REST API ve Java ile yazım denetimi yapma

Bing Yazım Denetimi REST API ilk çağrlarınızı yapmak için bu hızlı başlangıcı kullanın. Bu basit Java uygulaması, API 'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. 

Bu uygulama Java 'da yazılsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingSpellCheck.java)' da kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* Java Development Kit (JDK) 7 veya üzeri.

* [Gson-2.8.5. jar](https://libraries.io/maven/com.google.code.gson%3Agson) veya en güncel [gson](https://github.com/google/gson) sürümünü içeri aktarın. Komut satırı yürütme için, `.jar` ana sınıfla Java klasörünüze ekleyin.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Uygulama oluşturma ve başlatma

1. Seçtiğiniz bir sınıf adı ile en sevdiğiniz IDE veya düzenleyicide yeni bir Java projesi oluşturun ve ardından aşağıdaki paketleri içeri aktarın:

    ```java
    import java.io.*;
    import java.net.*;
    import com.google.gson.*;
    import javax.net.ssl.HttpsURLConnection;
    ```

2. API uç noktasının ana bilgisayar, yol ve abonelik anahtarınız için değişkenler oluşturun. Ardından, Pazar için değişkenler, yazım denetimi yapmak istediğiniz metin ve yazım denetimi modu için bir dize oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```java
    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    static String key = "<ENTER-KEY-HERE>";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";
    ```

## <a name="create-and-send-an-api-request"></a>API isteği oluşturma ve gönderme

1. `check()`API isteği oluşturmak ve göndermek için adlı bir işlev oluşturun. Bu işlev içinde, sonraki adımlarda belirtilen kodu ekleyin. İstek parametreleri için bir dize oluşturun:

   1. Pazar kodunuzu `mkt` parametreye, `=` işleçle atayın. 

   1. `mode`Parametresini `&` işleçle ekleyin ve ardından yazım denetimi modunu atayın. 

   ```java
   public static void check () throws Exception {
       String params = "?mkt=" + mkt + "&mode=" + mode;
      // add the rest of the code snippets here (except prettify() and main())...
   }
   ```

2. Uç nokta konağını, yolunu ve Parameters dizesini birleştirerek bir URL oluşturun. Yeni bir `HttpsURLConnection` nesne oluşturun.

    ```java
    URL url = new URL(host + path + params);
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    ```

3. URL 'ye bir bağlantı açın. İstek parametrelerini olarak ayarlayın `POST` ve ekleyin. Abonelik anahtarınızı üstbilgiye eklediğinizden emin olun `Ocp-Apim-Subscription-Key` .

    ```java
    connection.setRequestMethod("POST");
    connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
    connection.setDoOutput(true);
    ```

4. Yeni bir `DataOutputStream` nesne oluşturun ve ISTEğI API 'ye gönderin.

    ```java
        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();
    ```

## <a name="format-and-read-the-api-response"></a>API yanıtını biçimlendirme ve okuma

1. Sınıfını, `prettify()` daha okunabilir bir çıktı IÇIN JSON olarak biçimlendiren sınıfınıza ekleyin.

    ``` java
    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    ```

1. Bir oluşturun `BufferedReader` ve API 'den yanıtı okuyun. Bunu konsola yazdır.
    
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

Uygulamanızın ana işlevinde, `check()` daha önce oluşturduğunuz yöntemi çağırın.
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

Projenizi derleyin ve çalıştırın. Komut satırını kullanıyorsanız, uygulamayı derlemek ve çalıştırmak için aşağıdaki komutları kullanın:

1. Uygulamayı oluşturun:

   ```bash
   javac -classpath .;gson-2.2.2.jar\* <CLASS_NAME>.java
   ```

2. Uygulamayı çalıştırın:

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
- [Bing Yazım Denetimi API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
