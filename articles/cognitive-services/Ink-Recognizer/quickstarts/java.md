---
title: 'Hızlı Başlangıç: Mürekkep tanıyıcı REST API ve Java ile dijital mürekkep tanıma'
titleSuffix: Azure Cognitive Services
description: Dijital mürekkep vuruşlarını tanımayı başlatmak için mürekkep tanıyıcı API 'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 09/23/2019
ms.author: aahi
ms.openlocfilehash: 36ff0fe4550b140a722ed25f4e372f7c88581211
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212693"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Hızlı Başlangıç: Mürekkep tanıyıcı REST API ve Java ile dijital mürekkep tanıma

Mürekkep tanıyıcı API 'sini dijital mürekkep vuruşları üzerinde kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu Java uygulaması JSON biçimli mürekkep konturu verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama Java 'da yazıldığı sırada, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Genellikle, API 'YI bir dijital mürekkep uygulamasının içinden çağıracağınızdan. Bu hızlı başlangıç, bir JSON dosyasından aşağıdaki el yazısı örneği için mürekkep konturu verileri gönderir.

![El yazısı metin görüntüsü](../media/handwriting-sample.jpg)

Bu hızlı başlangıç için kaynak kodu [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904)' da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Java&trade; Development Kit (JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üzeri.

- Bu kitaplıkları Maven deposundan içeri aktar
    - [Java paketindeki JSON](https://mvnrepository.com/artifact/org.json/json)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paketi

- Bu hızlı başlangıç için örnek mürekkep konturu verileri [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-ink-strokes.json)'da bulunabilir.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Aşağıdaki uç noktayı mürekkep tanıyıcı kaynağınız için oluşturulan bir ile değiştirin. API 'ye bağlanmak için mürekkebi bir tanıyıcı URI 'sine ekleyin.

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
    // Replace the dataPath string with a path to the JSON formatted ink stroke data file.
    static final String dataPath = "PATH_TO_INK_STROKE_DATA";
    
    static final String endpoint = "https://<your-custom-subdomain>.cognitiveservices.azure.com";
    static final String inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı `sendRequest()` yeni bir işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.

2. API 'ye `CloseableHttpClient` istek gönderebilen bir nesne oluşturun. Uç noktanızı ve mürekkep tanıyıcı `HttpPut` URL 'sini birleştirerek isteği bir istek nesnesine gönderin.

3. Üstbilgiyi`Content-Type` `setHeader()` ayarlamakiçinisteğin`Ocp-Apim-Subscription-Key` işlevini kullanın ve abonelik anahtarınızı üstbilgiye ekleyin. `application/json`

4. Gönderilecek verilere isteğin `setEntity()` işlevini kullanın.   

5. İsteği göndermek ve bir `execute()` `CloseableHttpResponse` nesnesine kaydetmek için istemcinin işlevini kullanın. 

6. Yanıt içeriğini `HttpEntity` depolamak için bir nesne oluşturun. İçeriğini ile `getEntity()`alın. Yanıt boş değilse, döndürün.
    
    ```java
    static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPut request = new HttpPut(endpoint + apiAddress);
            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            request.setEntity(new StringEntity(requestData));
            try (CloseableHttpResponse response = client.execute(request)) {
                HttpEntity respEntity = response.getEntity();
                if (respEntity != null) {
                    return EntityUtils.toString(respEntity, "utf-8");
                }
            } catch (Exception respEx) {
                respEx.printStackTrace();
            }
        } catch (IOException ex) {
            System.err.println("Exception recognizing ink: " + ex.getMessage());
            ex.printStackTrace();
        }
        return null;
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönder

Mürekkep vuruşu verilerinizi tanımak `recognizeInk()` için adlı bir yöntem oluşturun. Uç nokta, URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemiçağırın.`sendRequest()` Sonucu alın ve konsola yazdırın.

```java
static void recognizeInk(String requestData) {
    System.out.println("Sending an ink recognition request");
    String result = sendRequest(inkRecognitionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Dijital mürekkep verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, isteklere eklenecek verileri içeren JSON dosyasını okuyun.

2. Yukarıda oluşturulan mürekkep tanıma işlevini çağırın.
    
    ```java
    public static void main(String[] args) throws Exception {
        String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
        recognizeInk(requestData);
    }
    ```

## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırma ve yanıtı görüntüleme

Uygulamayı çalıştırın. JSON biçiminde başarılı bir yanıt döndürülür. Ayrıca, [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-response.json)'da JSON yanıtını bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API 'sinin dijital bir mürekkep uygulamasında nasıl çalıştığını görmek için GitHub 'da aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
