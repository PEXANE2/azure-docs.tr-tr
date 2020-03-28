---
title: 'Hızlı başlatma: Azure REST API ve Node.js ile görüntüdeki yüzleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak için Node.js içeren Azure Yüz REST API'sini kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 12/06/2019
ms.author: pafarley
ms.openlocfilehash: 7897c1602a8b9ed6a1fea68e80d57cd5bae1c004
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76165959"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-nodejs"></a>Hızlı başlatma: Face REST API ve Node.js kullanarak görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, görüntüdeki insan yüzlerini algılamak için Node.js içeren Azure Yüz REST API'sini kullanırsınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api)ücretsiz deneme abonelik anahtarı alabilirsiniz. Veya Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin.
- Visual Studio Code gibi bir kod [düzenleyicisi](https://code.visualstudio.com/download)

## <a name="set-up-the-node-environment"></a>Düğüm ortamını ayarlama

Projenizi oluşturmak istediğiniz klasöre gidin ve yeni bir dosya, *facedetection.js*oluşturun. Ardından modülü `requests` bu projeye yükleyin. Bu, komut dosyalarınızın HTTP isteklerini yapmasına olanak tanır.

```shell
npm install request --save
```

## <a name="write-the-nodejs-script"></a>Düğüm.js komut dosyası yazın

Aşağıdaki kodu *facedetection.js'ye*yapıştırın. Bu alanlar, Yüz hizmetine nasıl bağlanılabildiğini ve giriş verilerinin nereden alınacağını belirtir. `subscriptionKey` Alanı abonelik anahtarınızın değeriyle güncelleştirmeniz ve `uriBase` doğru bitiş noktası dizesini içermesi için dizeyi değiştirmeniz gerekir. Alanı kendi giriş `imageUrl` resminizi işaret etmek için değiştirmek isteyebilirsiniz.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

```javascript
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase = 'https://<My Endpoint String>.com/face/v1.0/detect';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg';
```

Ardından, Face API'yi aramak ve giriş görüntüsünden yüz özniteliği verilerini almak için aşağıdaki kodu ekleyin. Alan, `returnFaceAttributes` alınacak öznitelikleri n için belirtir. Bu dizeyi, amacınıza bağlı olarak değiştirmek isteyebilirsiniz.


```javascript
// Request parameters.
const params = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,headPose,smile,facialHair,glasses,' +
        'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="save-and-run-the-script"></a>Komut dosyasını kaydedin ve çalıştırın

Değişikliklerinizi yaptıktan sonra bir komut istemi açın ve dosyayı `node` komutla çalıştırın.

```
node facedetection.js
```

Konsol penceresinde JSON verisi olarak görüntülenen yüz bilgilerini görmeniz gerekir. Örnek:

```json
[
  {
    "faceId": "ae8952c1-7b5e-4a5a-a330-a6aa351262c9",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "b1bb3cbe-5a73-4f8d-96c8-836a5aca9415",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, görüntüdeki yüzleri algılamak ve özniteliklerini döndürmek için Azure Face hizmetini çağıran bir Düğüm.js komut dosyası yazdınız. Ardından, daha fazla bilgi edinmek için Yüz API başvuru belgelerini keşfedin.

> [!div class="nextstepaction"]
> [Yüz API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
