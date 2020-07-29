---
title: Bing Varlık Arama Java istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.custom: devx-track-java
ms.author: aahi
ms.openlocfilehash: f69b9b989a93949f9a0441676c81af7480fb968f
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87375243"
---
Java için Bing Varlık Arama istemci kitaplığıyla varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch)' da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* Java için Bing Varlık Arama istemci kitaplığı

Maven, Gradle veya başka bir bağımlılık yönetim sistemini kullanarak Bing Varlık Arama istemci kitaplığı bağımlılıklarını yükler. Maven POM dosyası şu bildirimi gerektirir:

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

2. Abonelik anahtarınız için bir değişken oluşturun

    ```java
    String subscriptionKey = "your-key-here"
    ```

## <a name="create-a-search-client"></a>Arama İstemcisi Oluşturma

1. `dominantEntityLookup`API uç noktanızı ve sınıfının bir örneğini gerektiren istemciyi uygulayın `ServiceClientCredentials` . Aşağıdaki genel uç noktayı veya kaynak için Azure portal görüntülenmiş [özel alt etki alanı](../../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Uygulamak için `ServiceClientCredentials` aşağıdaki adımları izleyin:

   1. `applyCredentialsFilter()`parametresi olarak bir nesnesi ile işlevi geçersiz kılın `OkHttpClient.Builder` . 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. İçinde `applyCredentialsFilter()` , çağırın `builder.addNetworkInterceptor()` . Yeni bir `Interceptor` nesne oluşturun ve `intercept()` yöntemini bir dinleyici nesnesi alacak şekilde geçersiz kılın `Chain` .

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

   3. İşlevi içinde `intercept` , isteğiniz için değişkenler oluşturun. `Request.Builder()`İsteğinizi derlemek için kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve `chain.proceed()` istek nesnesine dönün.
            
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

1. Abonelik anahtarınızla arama istemcisinin yeni bir örneğini oluşturun. `client.entities().search()`arama sorgusu için bir arama isteği göndermek `satya nadella` ve bir yanıt almak için kullanın. 
    
    ```java
    EntitySearchAPIImpl client = getClient(subscriptionKey);
    SearchResponseInner entityData = client.entities().search(
            "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
    ```

1. Herhangi bir varlık döndürülürse, bunları bir listeye dönüştürün. Aralarında yineleme yapın ve baskın varlığı yazdırın.

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
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md)
