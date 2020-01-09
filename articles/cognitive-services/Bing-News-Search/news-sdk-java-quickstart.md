---
title: "Hızlı başlangıç: Java için SDK 'Yı kullanarak haber araması gerçekleştirme-Bing Haber Arama"
titleSuffix: Azure Cognitive Services
description: Java için Bing Haber Arama SDK 'yı kullanarak haberleri aramak ve yanıtı işlemek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: b8c2ca3bf919d70123b7b5b6d13c0301e0c1fd13
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75383587"
---
# <a name="quickstart-search-for-news-with-the-bing-news-search-sdk-for-java"></a>Hızlı başlangıç: Java için Bing Haber Arama SDK ile haberleri arama

Java için Bing Haber Arama SDK 'Sı ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

Maven, Gradle veya başka bir bağımlılık yönetim sistemini kullanarak Bing Haber Arama SDK bağımlılıklarını yükler. Maven POM dosyası şu bildirimi gerektirir:

```xml
    <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
    </dependencies>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

```java
import com.microsoft.azure.cognitiveservices.newssearch.*;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsInner;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsSearchAPIImpl;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.TrendingTopicsInner;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import java.io.IOException;
```

## <a name="create-a-search-client-and-store-credentials"></a>Arama İstemcisi Oluşturma ve kimlik bilgilerini depolama

1. Yeni bir `NewsSearchAPIImpl` arama istemcisi döndüren `getClient()` adlı bir yöntem oluşturun. Yeni`NewsSearchAPIImpl` nesnesi için ilk parametre olarak uç noktanızı ve kimlik bilgilerinizi depolamak için yeni bir `ServiceClientCredentials` nesnesini ekleyin. 

    ```java
    public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
        return new NewsSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                });
    }
    ```

2. `ServiceClientCredentials` nesnesini oluşturmak için `applyCredentialsFilter()` işlevini geçersiz kılın. Yöntemine bir `OkHttpClient.Builder` geçirin ve SDK çağrısı için kimlik bilgilerinizi oluşturmak üzere oluşturucunun `addNetworkInterceptor()` yöntemini kullanın.

    ```java
    new ServiceClientCredentials() {
        @Override
        public void applyCredentialsFilter(OkHttpClient.Builder builder) {
            builder.addNetworkInterceptor(
                    new Interceptor() {
                        @Override
                        public Response intercept(Chain chain) throws IOException {
                            Request request = null;
                            Request original = chain.request();
                            // Request customization: add request headers.
                            Request.Builder requestBuilder = original.newBuilder()
                                    .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                            request = requestBuilder.build();
                            return chain.proceed(request);
                        }
                    });
        }
    });
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma

1. `getClient()` çağıran ve Bing Haber Arama hizmetine bir arama isteği gönderen bir yöntem oluşturun. Bu aramayı *Pazar* ve *sayı* parametreleriyle filtreleyin, ardından ilk haber sonucuyla ilgili bilgileri YAZDıRıN: ad, URL, Yayın tarihi, açıklama, sağlayıcı adı ve aramanızın tahmini eşleşmelerin toplam sayısı.

    ```java
    public static void newsSearch(String subscriptionKey)
    {
        NewsSearchAPIImpl client = getClient(subscriptionKey);
        String searchTerm = "Quantum Computing";
    
        NewsInner newsResults = client.searchs().list(searchTerm, null, null, null,
                null, null, 100, null, "en-us",
                null, null, null, null, null,
                null, null);
    
        if (newsResults.value().size() > 0)
        {
            NewsArticle firstNewsResult = newsResults.value().get(0);
    
            System.out.println(String.format("TotalEstimatedMatches value: %d", newsResults.totalEstimatedMatches()));
            System.out.println(String.format("News result count: %d", newsResults.value().size()));
            System.out.println(String.format("First news name: %s", firstNewsResult.name()));
            System.out.println(String.format("First news url: %s", firstNewsResult.url()));
            System.out.println(String.format("First news description: %s", firstNewsResult.description()));
            System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
            System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
        }
        else
        {
            System.out.println("Couldn't find news results!");
        }
    
    }
    
    ```

2. Kodu yürütmek için arama yönteminizi bir `main()` yöntemine ekleyin.

    ```java 
    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch(subscriptionKey);
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
