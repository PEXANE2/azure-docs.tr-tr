---
title: Bing Video Arama Java istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.custom: devx-track-java
ms.author: aahi
ms.openlocfilehash: 2b3d4993406f150b2983d4d820f7d070b5de1e96
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374662"
---
Java için Bing Video Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu, ek açıklamalar ve özellikler ile [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingVideoSearch)'da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)

* [Gson kitaplığı](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

Maven, Gradle veya başka bir bağımlılık yönetim sistemini kullanarak Bing Video Arama istemci kitaplığı bağımlılıklarını yükler. Maven POM dosyası şu bildirimi gerektirir:

```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-videosearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies> 
```

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma


Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

```java
    import com.microsoft.azure.cognitiveservices.videosearch.*;
    import com.microsoft.azure.cognitiveservices.videosearch.VideoObject;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List; 
```

## <a name="create-a-search-client"></a>Arama İstemcisi Oluşturma

1. `VideoSearchAPIImpl`API uç noktanızı ve sınıfının bir örneğini gerektiren istemciyi uygulayın `ServiceClientCredentials` .

    ```java
    public static VideoSearchAPIImpl getClient(final String subscriptionKey) {
        return new VideoSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
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

## <a name="send-a-search-request-and-receive-the-response"></a>Arama isteği gönderme ve yanıtı alma 

1. `VideoSearch()`Abonelik anahtarınızı bir dize olarak alan adlı bir işlev oluşturun. Daha önce oluşturulan arama istemcisinin örneğini oluşturun.
    
    ```java
    public static void VideoSearch(String subscriptionKey){
        VideoSearchAPIImpl client = VideoSDK.getClient(subscriptionKey);
        //...
    }
    ```
2. İçinde `VideoSearch()` , arama terimi olarak, istemcisini kullanarak bir video arama isteği gönderin `SwiftKey` . Video Arama API 'SI bir sonuç döndürdüğünden, ilk sonucu alın ve kimliğini, adını ve URL 'sini, döndürülen toplam video sayısı ile birlikte yazdırın. 
    
    ```java
    VideosInner videoResults = client.searchs().list("SwiftKey");

    if (videoResults == null){
        System.out.println("Didn't see any video result data..");
    }
    else{
        if (videoResults.value().size() > 0){
            VideoObject firstVideoResult = videoResults.value().get(0);

            System.out.println(String.format("Video result count: %d", videoResults.value().size()));
            System.out.println(String.format("First video id: %s", firstVideoResult.videoId()));
            System.out.println(String.format("First video name: %s", firstVideoResult.name()));
            System.out.println(String.format("First video url: %s", firstVideoResult.contentUrl()));
        }
        else{
            System.out.println("Couldn't find video results!");
        }
    }
    ```

3. Main yönteminizin arama yöntemini çağırın.

    ```java
    public static void main(String[] args) {
        VideoSDK.VideoSearch("YOUR-SUBSCRIPTION-KEY");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı Web uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video Arama API'si nedir?](../../overview.md)
* [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
