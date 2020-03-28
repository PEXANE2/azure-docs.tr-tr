---
title: Bing Haberler Arama Java istemci kitaplığı quickstart
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: 76a752bc108158c998d13a548da535d5fb1d5ee2
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79503877"
---
Java için Bing Haberler Arama istemci kitaplığı ile haber aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Aramaçoğu programlama diliyle uyumlu bir REST API'si olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

Bing Haber Arama istemci kitaplığı bağımlılıklarını Maven, Gradle veya başka bir bağımlılık yönetim sistemini kullanarak yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
    <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
    </dependencies>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

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

## <a name="create-a-search-client-and-store-credentials"></a>Arama istemcisi oluşturma ve kimlik bilgilerini depolama

1. Yeni `NewsSearchAPIImpl` bir `getClient()` arama istemcisi döndüren bir yöntem oluşturun. Bitiş noktanızı yeni`NewsSearchAPIImpl` nesne için ilk parametre olarak `ServiceClientCredentials` ve kimlik bilgilerinizi depolayacak yeni bir nesne ekleyin. 

    ```java
    public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
        return new NewsSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                });
    }
    ```

2. Nesneyi `ServiceClientCredentials` oluşturmak için `applyCredentialsFilter()` işlevi geçersiz kılın. Yönteme `OkHttpClient.Builder` bir geçiş ve istemci kitaplık çağrısı için kimlik bilgilerinizi oluşturmak için oluşturucu `addNetworkInterceptor()` yöntemini kullanın.

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

1. Bing Haber Arama `getClient()` hizmetine arama çağrısı yapan ve arama isteği gönderen bir yöntem oluşturun. Aramayı *pazara* göre filtreleyin ve parametreleri *sayar,* ardından ilk haber sonucu hakkında bilgi yazdırın: ad, URL, yayın tarihi, açıklama, sağlayıcı adı ve aramanız için tahmini eşleşmelerin toplam sayısı.

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

2. Kodu yürütmek için `main()` arama yönteminizi bir yönteme ekleyin.

    ```java 
    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch(subscriptionKey);
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-news-search-single-page-app.md)
