---
title: 'Hızlı başlangıç: Java için SDK ile varlıkları arama-Bing Varlık Arama'
titleSuffix: Azure Cognitive Services
description: Java için Bing Varlık Arama SDK 'Sı ile varlıkları aramak için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 12/11/2019
ms.author: aahi
ms.openlocfilehash: 49e3492bae1dfe248e07227caff11a6d00e59c08
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76716482"
---
# <a name="quickstart-send-a-search-request-with-the-bing-entity-search-sdk-for-java"></a>Hızlı başlangıç: Java için Bing Varlık Arama SDK ile bir arama isteği gönderme

Java için Bing Varlık Arama SDK ile varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* Java için Bing Varlık Arama SDK 'Sı

Bing Varlık Arama SDK'sı bağımlılık dosyalarını Maven, Gradle veya başka bir bağımlılık dosyası yönetim sistemini kullanarak yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
<dependency>
  <groupId>com.microsoft.azure.cognitiveservices</groupId>
  <artifactId>azure-cognitiveservices-entitysearch</artifactId>
  <version>1.0.2</version>
</dependency>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]


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

1. API uç noktanızı gerektiren `dominantEntityLookup` istemcisini ve `ServiceClientCredentials` sınıfının bir örneğini uygulayın. Aşağıdaki genel uç noktayı veya kaynak için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    `ServiceClientCredentials`uygulamak için aşağıdaki adımları izleyin:

   1. `applyCredentialsFilter()` işlevini parametre olarak `OkHttpClient.Builder` bir nesne ile geçersiz kılın. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. `applyCredentialsFilter()`içinde `builder.addNetworkInterceptor()`çağırın. Yeni bir `Interceptor` nesnesi oluşturun ve `Chain` bir dinleyici nesnesi almak için `intercept()` metodunu geçersiz kılın.

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

   3. `intercept` işlevi içinde, isteğiniz için değişkenler oluşturun. İsteğinizi derlemek için `Request.Builder()` kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgisine ekleyin ve istek nesnesine `chain.proceed()` döndürün.
            
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

1. Abonelik anahtarınızla arama istemcisinin yeni bir örneğini oluşturun. Arama sorgusu `satya nadella`için bir arama isteği göndermek ve bir yanıt almak için `client.entities().search()` kullanın. 
    
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
> [Tek sayfalı Web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../overview.md )
