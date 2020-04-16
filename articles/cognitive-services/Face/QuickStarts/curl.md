---
title: 'Hızlı başlatma: Azure REST API ve cURL ile görüntüdeki yüzleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak için cURL'li Azure Yüz REST API'sini kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 7e7eb9ba942cd29db824388c02b2933c533c4b45
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403029"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-curl"></a>Hızlı başlatma: Yüz REST API ve cURL kullanarak görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, görüntüdeki insan yüzlerini algılamak için cURL'li Azure Yüz REST API'sini kullanırsınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api)ücretsiz deneme abonelik anahtarı alabilirsiniz. Veya Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin.

## <a name="write-the-command"></a>Komutu yazın
 
Yüz API'sini aramak ve görüntüden yüz özniteliği verileri almak için aşağıdaki gibi bir komut kullanırsınız. İlk olarak, kodu çalıştırabilmeniz için komutun belirli bölümlerinde değişiklik yapmanız gereken bir metin düzenleyicisine&mdash;kopyalayın.

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://<My Endpoint String>.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="subscription-key"></a>Abonelik anahtarı
Geçerli `<Subscription Key>` Face abonelik anahtarınızla değiştirin.

### <a name="face-endpoint-url"></a>Yüz uç noktası URL'si

URL, `https://<My Endpoint String>.com/face/v1.0/detect` sorguiçin Azure Yüz bitiş noktasını gösterir. Bu URL'nin ilk bölümünü abonelik anahtarınıza karşılık gelen bitiş noktasıyla eşleşecek şekilde değiştirmeniz gerekebilir.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

### <a name="url-query-string"></a>URL sorgu dizesi

Face endpoint URL'sinin sorgu dizesi, alınacak özniteliklerin hangi sini olduğunu belirtir. Bu dizeyi, amacınıza bağlı olarak değiştirmek isteyebilirsiniz.

```
?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise
```

### <a name="image-source-url"></a>Resim kaynak URL'si
Kaynak URL, giriş olarak kullanılacak resmi gösterir. Bunu, çözümlemek istediğiniz herhangi bir görüntüye işaret etmek için değiştirebilirsiniz.

```
https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg
``` 

## <a name="run-the-command"></a>Yeni bir “kurtarma VM’si” oluşturmak ve sorunlu VM’nin işletim sistemi diskini kurtarma VM’sine veri diski olarak takmak için

Değişikliklerinizi yaptıktan sonra bir komut istemi açın ve yeni komutu girin. Konsol penceresinde JSON verisi olarak görüntülenen yüz bilgilerini görmeniz gerekir. Örneğin:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
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
      }
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak ve özniteliklerini döndürmek için Azure Face hizmetini çağıran bir cURL komutu yazdınız. Ardından, daha fazla bilgi edinmek için Yüz API başvuru belgelerini keşfedin.

> [!div class="nextstepaction"]
> [Yüz API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
