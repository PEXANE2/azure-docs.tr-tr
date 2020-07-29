---
title: 'Hızlı başlangıç: REST API ve Java ile uzak görüntüyü çözümleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ile Görüntü İşleme API’si kullanarak bir görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18, seo-java-august2019, seo-java-september2019, devx-track-java
ms.openlocfilehash: 78781fbd565b53a1446e5a8d92b5bf15b26e9e40
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323358"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-and-java"></a>Hızlı başlangıç: Görüntü İşleme REST API ve Java kullanarak uzak görüntüyü çözümleme

Bu hızlı başlangıçta, Java ve Görüntü İşleme REST API kullanarak görsel özellikleri ayıklamak için uzaktan depolanan bir görüntüyü çözümlüreceğiz. [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yöntemiyle, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Java &trade; Platform, Standard Edition geliştirme seti 7 veya 8](https://aka.ms/azure-jdks) (JDK 7 veya 8)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Tercih ettiğiniz IDE veya düzenleyicide bir Java projesi oluşturun. Seçenek yoksa, Java projesini bir komut satırı uygulaması şablondan oluşturun.
1. Aşağıdaki kitaplıkları Java projenize içeri aktarın. Maven kullanıyorsanız, her bir kitaplık için Maven koordinatları sağlanır.
   - [Apache HTTP istemcisi](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Apache HTTP çekirdek](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [JSON kitaplığı](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Aşağıdaki `import` deyimlerini projeniz için `Main` ortak sınıfını içeren dosyaya ekleyin.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.json.JSONObject;
   ```

1. `AnalyzeImage`Ortak sınıfı aşağıdaki kodla değiştirin.
1. İsteğe bağlı olarak `imageToAnalyze` değerini, analiz etmek istediğiniz başka bir görüntünün URL’si ile değiştirin.

```java
public class AnalyzeImage {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    private static String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    private static String endpoint = System.getenv("COMPUTER_VISION_ENDPOINT");

    private static final String uriBase = endpoint + "vision/v3.0/analyze";
    private static final String imageToAnalyze =
            "https://upload.wikimedia.org/wikipedia/commons/" +
            "1/12/Broadway_and_Times_Square_by_night.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();

        try {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Call the REST API method and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="compile-and-run-the-program"></a>Programı derleme ve çalıştırma

1. Kaydedin, ardından Java projesini derleyin.
1. Bir IDE kullanıyorsanız, `Main` yöntemini çalıştırın.

Alternatif olarak, programı bir komut satırı penceresinden çalıştırıyorsanız, aşağıdaki komutları çalıştırın. Bu komutlar, kitaplıklarınız aynı klasörde olan adlı bir klasörde yer alır `libs` `Main.java` ; Aksi takdirde, `libs` kitaplıklarınızın yoluyla değiştirmeniz gerekir.

1. Dosyayı derleyin `Main.java` .

    ```bash
    javac -cp ".;libs/*" Main.java
    ```

1. Programı çalıştırın. Soru-Cevap Oluşturma API'sine KB oluşturma isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt komut satırı penceresine yazdırılır.

    ```bash
    java -cp ".;libs/*" Main
    ```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek uygulama aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp konsol penceresinde görüntüler:

```json
REST Response:

{
  "metadata": {
    "width": 1826,
    "format": "Jpeg",
    "height": 2436
  },
  "color": {
    "dominantColorForeground": "Brown",
    "isBWImg": false,
    "accentColor": "B74314",
    "dominantColorBackground": "Brown",
    "dominantColors": ["Brown"]
  },
  "requestId": "bbffe1a1-4fa3-4a6b-a4d5-a4964c58a811",
  "description": {
    "captions": [{
      "confidence": 0.8241405091548035,
      "text": "a group of people on a city street filled with traffic at night"
    }],
    "tags": [
      "outdoor",
      "building",
      "street",
      "city",
      "busy",
      "people",
      "filled",
      "traffic",
      "many",
      "table",
      "car",
      "group",
      "walking",
      "bunch",
      "crowded",
      "large",
      "night",
      "light",
      "standing",
      "man",
      "tall",
      "umbrella",
      "riding",
      "sign",
      "crowd"
    ]
  },
  "categories": [{
    "score": 0.625,
    "name": "outdoor_street"
  }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan bir Java Swing uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Java Öğreticisi](../Tutorials/java-tutorial.md)
