---
title: 'Hızlı başlangıç: Azure REST API ve Java ile görüntüdeki yüzeyleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir görüntüdeki yüzeyleri algılamak için Java ile REST API Azure yüz kullanacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/05/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: fbe62cf00422710e18a6b112adc08f19ea03177b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91858362"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Hızlı Başlangıç: REST API ve Java kullanarak bir görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, bir görüntüdeki insan yüzlerini algılamak için Java ile REST API Azure yüz 'yi kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Yüz Tanıma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Dilediğiniz Java IDE.

## <a name="create-the-java-project"></a>Java projesi oluşturma

1. IDE 'nize yeni bir komut satırı Java uygulaması oluşturun ve **Main** yöntemine sahip bir **ana** sınıf ekleyin.
1. Aşağıdaki kitaplıkları Java projenize içeri aktarın. Maven kullanıyorsanız, her bir kitaplık için Maven koordinatları sağlanır.
   - [Apache HTTP istemcisi](https://hc.apache.org/downloads.cgi) (org. Apache. httpcomponents: HttpClient: 4.5.6)
   - [Apache HTTP Core](https://hc.apache.org/downloads.cgi) (org. Apache. httpcomponents: httpcore: 4.4.10)
   - [JSON kitaplığı](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons günlüğü](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (Commons günlük: Commons-Logging: 1.1.2)

## <a name="add-face-detection-code"></a>Yüz algılama kodu ekleme

Projenizin ana sınıfını açın. Burada, görüntüleri yüklemek ve yüzeyleri algılamak için gereken kodu ekleyeceksiniz.

### <a name="import-packages"></a>Paketleri içeri aktarma

Aşağıdaki `import` deyimlerini dosyanın en üstüne ekleyin.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="dependencies":::

### <a name="add-essential-fields"></a>Gerekli alanları Ekle

**Ana** sınıfı aşağıdaki kodla değiştirin. Bu veriler, yüz hizmetine bağlanmayı ve giriş verilerinin nereden alınacağını belirtir. `subscriptionKey`Alanı abonelik anahtarınızın değeriyle güncelleştirmeniz ve `uriBase` dizeyi doğru uç nokta dizesini içerecek şekilde değiştirmeniz gerekir. Ayrıca `imageWithFaces` değeri, farklı bir görüntü dosyasına işaret eden bir yola ayarlamak isteyebilirsiniz.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

`faceAttributes`Alanı yalnızca belirli öznitelik türlerinin bir listesidir. Algılanan yüzler hakkında hangi bilgilerin alınacağı belirler.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="environment":::

### <a name="call-the-face-detection-rest-api"></a>Yüz algılamayı çağırın REST API

**Ana** yöntemi aşağıdaki kodla ekleyin. Uzak görüntüdeki yüz bilgilerini algılamak için Yüz Tanıma API'si bir REST çağrısı oluşturur ( `faceAttributes` dize hangi yüz özniteliklerini almak üzere belirtir). Ardından, çıkış verilerini bir JSON dizesine yazar.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="main":::

### <a name="parse-the-json-response"></a>JSON yanıtını Ayrıştır

Önceki kodun hemen altına, döndürülen JSON verilerini konsola yazdırmadan önce daha kolay okunabilir bir biçime dönüştüren aşağıdaki bloğu ekleyin. Son olarak, try-catch bloğunu, **Main** yöntemini ve **ana** sınıfı kapatın.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="print":::

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Kodu derleyin ve çalıştırın. Başarılı bir yanıt, yüz verilerini konsol penceresinde kolay okunabilir JSON biçiminde görüntüler. Örnek:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  }
}]
```

## <a name="extract-face-attributes"></a>Yüz özniteliklerini Ayıkla
 
Yüz özniteliklerini ayıklamak için, algılama modeli 1 ' i kullanın ve `returnFaceAttributes` sorgu parametresini ekleyin.

```java
builder.setParameter("detectionModel", "detection_01");
builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise");
```

Yanıt şimdi yüz öznitelikleri içeriyor. Örnek:

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

Bu hızlı başlangıçta, bir görüntüdeki yüzeyleri algılamak ve özniteliklerini döndürmek için Azure Yüz Tanıma API'si REST çağrılarını kullanan basit bir Java konsol uygulaması oluşturdunuz. Daha sonra, desteklenen senaryolar hakkında daha fazla bilgi edinmek için Yüz Tanıma API'si başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
