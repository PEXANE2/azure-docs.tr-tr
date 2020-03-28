---
title: Bing Entity Search Java istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 8c987aa14e922573d01aa35fab609edf01e109b4
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79136777"
---
Java için Bing Entity Search istemci kitaplığı olan varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Entity Search çoğu programlama diliyle uyumlu bir REST API'sine sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirme Kiti(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* Java için Bing Varlık Arama istemci kitaplığı

Maven, Gradle veya başka bir bağımlılık yönetim sistemi kullanarak Bing Varlık Arama istemcisi kitaplık bağımlılıklarını yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
<dependency>
  <groupId>com.microsoft.azure.cognitiveservices</groupId>
  <artifactId>azure-cognitiveservices-entitysearch</artifactId>
  <version>1.0.2</version>
</dependency>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import com.microsoft.azure.cognitiveservices.entitysearch.*;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.EntitySearchAPIImpl;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.SearchResponseInner;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List;
    ```

2. Abonelik anahtarınız için bir değişken oluşturma

    ```java
    String subscriptionKey = "your-key-here"
    ```

## <a name="create-a-search-client"></a>Arama istemcisi oluşturma

1. API `dominantEntityLookup` bitiş noktanızı ve sınıfın bir örneğini `ServiceClientCredentials` gerektiren istemciyi uygulayın. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Uygulamak için `ServiceClientCredentials`aşağıdaki adımları izleyin:

   1. bir `OkHttpClient.Builder` nesne `applyCredentialsFilter()` parametre olarak işlevi geçersiz kılın. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. Içinde `applyCredentialsFilter()`, `builder.addNetworkInterceptor()`çağrı . Yeni `Interceptor` bir nesne oluşturun ve `intercept()` bir `Chain` durdurucu nesnealmak için yöntemini geçersiz kılın.

       ```java
       //...
       builder.addNetworkInterceptor(
           new Interceptor() {
               @Override
               public Response intercept(Chain chain) throws IOException {
               //...    
               }
           });
       ///...
       ```

   3. `intercept` İşlev içinde, isteğiniz için değişkenler oluşturun. İsteğinizi oluşturmak için kullanın. `Request.Builder()` Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin `chain.proceed()` ve istek nesnesini döndürün.
            
       ```java
       //...
       public Response intercept(Chain chain) throws IOException {
           Request request = null;
           Request original = chain.request();
           Request.Builder requestBuilder = original.newBuilder()
                   .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
           request = requestBuilder.build();
           return chain.proceed(request);
       }
       //...
       ```
## <a name="send-a-request-and-receive-a-response"></a>İstek gönderme ve yanıt alma

1. Abonelik anahtarınızla arama istemcisinin yeni bir örneğini oluşturun. arama `client.entities().search()` sorgusu `satya nadella`için arama isteği göndermek ve yanıt almak için kullanın. 
    
    ```java
    EntitySearchAPIImpl client = getClient(subscriptionKey);
    SearchResponseInner entityData = client.entities().search(
            "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
    ```

1. Herhangi bir varlık döndürüldüyse, bunları bir listeye dönüştürün. Onlar aracılığıyla iterate ve baskın varlık yazdırın.

    ```java
    if (entityData.entities().value().size() > 0){
        // Find the entity that represents the dominant entity
        List<Thing> entries = entityData.entities().value();
        Thing dominateEntry = null;
        for(Thing thing : entries) {
            if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                System.out.println("\r\nSearched for \"Satya Nadella\" and found a dominant entity with this description:");
                System.out.println(thing.description());
                break;
            }
        }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalık bir web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md)
