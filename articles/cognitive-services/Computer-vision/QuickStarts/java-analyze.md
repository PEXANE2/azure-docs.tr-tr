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
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: seodec18, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 526cc8ca002b3b6c2821eb842b0c6a7794f2cd33
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74968995"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-and-java"></a>Hızlı başlangıç: Görüntü İşleme REST API ve Java kullanarak uzak görüntüyü çözümleme

Bu hızlı başlangıçta, Java ve Görüntü İşleme REST API kullanarak görsel özellikleri ayıklamak için uzaktan depolanan bir görüntüyü analiz edersiniz. [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yöntemiyle, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [Java&trade; Platform, Standard Edition Geliştirme Seti 7 veya 8](https://aka.ms/azure-jdks) (JDK 7 veya 8) yüklü olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)'den ücretsiz bir deneme anahtarı edinebilirsiniz. Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Ardından, sırasıyla `COMPUTER_VISION_SUBSCRIPTION_KEY` ve `COMPUTER_VISION_ENDPOINT`adlı anahtar ve hizmet uç noktası dizesi için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Sık kullandığınız IDE veya düzenleyicide yeni bir Java projesi oluşturun. Seçenek yoksa, Java projesini bir komut satırı uygulaması şablondan oluşturun.
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

1. `Main` ortak sınıfını aşağıdaki kodla değiştirin.
1. İsteğe bağlı olarak `imageToAnalyze` değerini, analiz etmek istediğiniz başka bir görüntünün URL’si ile değiştirin.

```java
public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    String endpoint = ("COMPUTER_VISION_ENDPOINT");

    private static final String uriBase = endpoint + 
            "vision/v2.1/analyze";
    private static final String imageToAnalyze =
            "https://upload.wikimedia.org/wikipedia/commons/" +
                    "1/12/Broadway_and_Times_Square_by_night.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();

        try {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

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

Alternatif olarak, programı bir komut satırı penceresinden çalıştırıyorsanız, aşağıdaki komutları çalıştırın. Bu komutlar, kitaplıklarınızın `Main.java`aynı klasördeki `libs` adlı bir klasörde olduğunu varsayar; Aksi takdirde, `libs` kitaplıklarınızın yoluyla değiştirmeniz gerekir.

1. Dosyayı derleyin `Main.java`.

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
