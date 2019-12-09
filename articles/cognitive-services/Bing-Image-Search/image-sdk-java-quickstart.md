---
title: 'Hızlı başlangıç: görüntü arama-Java için Bing Resim Arama SDK'
titleSuffix: Azure Cognitive Services
description: Bu öğreticiyi API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Görüntü Arama SDK'sını kullanarak ilk görüntü aramanızı yapmak için kullanın. Bu basit Java uygulaması bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL’sini görüntüler.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 12/06/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: cb25f6be358ae8aed18bf392ad933eab87332dcb
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930607"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-for-java"></a>Hızlı Başlangıç: Java için Bing Resim Arama SDK'sı ile görüntü arama

Bu öğreticiyi API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Görüntü Arama SDK'sını kullanarak ilk görüntü aramanızı yapmak için kullanın. Bu basit Java uygulaması bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL’sini görüntüler.

Bu örneğin kaynak kodu, ek hata işleme ve açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart)’da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

[Java Geliştirme Seti](https://aka.ms/azure-jdks)’nin (JDK) en son sürümü

Maven, Gradle veya başka bir bağımlılık dosyası yönetim sistemini kullanarak Bing Resim Arama SDK’sı bağımlılık dosyalarını yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>1.0.1</version>
    </dependency>
 </dependencies>
```

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Java projesi oluşturun ve sınıf uygulamanıza aşağıdaki içeri aktarmaları ekleyin:

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. Ana yönteminizde abonelik anahtarınız ve arama teriminiz için değişkenler oluşturun. Ardından Bing Resim Arama istemcisinin örneğini oluşturun.

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## <a name="send-a-search-request-to-the-api"></a>API 'ye bir arama isteği gönder

1. `bingImages().search()` kullanarak, arama sorgusunu içeren HTTP isteğini gönderin. Yanıtı `ImagesModel` olarak kaydedin.

   ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## <a name="parse-and-view-the-result"></a>Sonucu ayrıştırma ve görüntüleme

Yanıtta döndürülen resim sonuçlarını ayrıştırın.
Yanıt, arama sonuçları içeriyorsa, ilk sonucu depolayın ve küçük resim URL 'SI, özgün URL gibi ayrıntılarını, döndürülen görüntülerin toplam sayısıyla birlikte yazdırın.  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
     }

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Azure Bilişsel Hizmetler SDK’sı için Java örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
