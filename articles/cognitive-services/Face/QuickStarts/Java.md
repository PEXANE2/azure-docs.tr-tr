---
title: 'Hızlı başlatma: Azure REST API ve Java ile görüntüdeki yüzleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak için Java içeren Azure Yüz REST API'sini kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 59c121f09858114cecfaeb45e257315d1ab92935
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403435"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Hızlı Başlangıç: REST API ve Java kullanarak bir görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, görüntüdeki insan yüzlerini algılamak için Java içeren Azure Yüz REST API'sini kullanırsınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api)ücretsiz deneme abonelik anahtarı alabilirsiniz. Veya Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin.
- Seçtiğiniz herhangi bir Java IDE.

## <a name="create-the-java-project"></a>Java projesini oluşturma

1. IDE'nizde yeni bir komut satırı Java uygulaması oluşturun ve **ana** yöntemle bir **Ana** sınıf ekleyin.
1. Aşağıdaki kitaplıkları Java projenize içeri aktarın. Maven kullanıyorsanız, her bir kitaplık için Maven koordinatları sağlanır.
   - [Apache HTTP istemcisi](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Apache HTTP çekirdek](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [JSON kitaplığı](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons günlüğü](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logging:commons-logging:1.1.2)

## <a name="add-face-detection-code"></a>Yüz algılama kodu ekleme

Projenizin ana sınıfını açın. Burada, görüntüleri yüklemek ve yüzleri algılamak için gereken kodu eklersiniz.

### <a name="import-packages"></a>Paketleri içeri aktarma

Aşağıdaki `import` ifadeleri dosyanın üstüne ekleyin.

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Temel alanlar ekleme

**Ana** sınıfı aşağıdaki kodla değiştirin. Bu veriler, Face hizmetine nasıl bağlanılacağını ve girdi verilerinin nereden alınacağını belirtir. `subscriptionKey` Alanı abonelik anahtarınızın değeriyle güncelleştirmeniz ve dizeyi `uriBase` doğru bitiş noktası dizesini içerecek şekilde değiştirmeniz gerekir. `imageWithFaces` Ayrıca, değeri farklı bir resim dosyasına işaret eden bir yola ayarlamak isteyebilirsiniz.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

Alan `faceAttributes` sadece belirli özniteliklerin bir listesidir. Algılanan yüzler hakkında hangi bilgilerin alınacağını belirtir.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    private static final String uriBase =
        "https://<My Endpoint String>.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>Yüz algılama REST API'yi arayın

Aşağıdaki kodu içeren **ana** yöntemi ekleyin. Uzak görüntüdeki yüz bilgilerini algılamak için Face API'ye `faceAttributes` REST çağrısı oluşturur (dize hangi yüz özniteliklerini alır) belirtir. Sonra çıktı verilerini JSON dizesine yazar.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();
```

### <a name="parse-the-json-response"></a>Ayrışdırış JSON yanıtı

Önceki kodun hemen altına, döndürülen JSON verilerini konsola yazdırmadan önce daha kolay okunabilir bir biçime dönüştüren aşağıdaki bloğu ekleyin. Son olarak, try-catch bloğunu, **ana** yöntemi ve **Ana** sınıfı kapatın.

```Java
            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Kodu derle ve çalıştır. Başarılı bir yanıt, Yüz verilerini konsol penceresinde kolayca okunabilir JSON formatında görüntüler. Örneğin:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak ve özniteliklerini döndürmek için Azure Yüz API'sine REST çağrıları kullanan basit bir Java konsolu uygulaması oluşturdunuz. Ardından, bir Android uygulamasında bu işlevsellikle nasıl daha fazlasını yapacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Öğretici: Yüzleri algılamak ve çerçevelemek için bir Android uygulaması oluşturun](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
