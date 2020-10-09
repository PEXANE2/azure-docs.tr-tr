---
title: 'Hızlı başlangıç: Azure REST API ve C ile görüntüdeki yüzeyleri algılama #'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir görüntüdeki yüzeyleri algılamak Için C# ile REST API Azure yüz kullanacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 1b8bcaf283e612b3ebe6d6b7bb5660e8b3179ad3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91858379"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-c"></a>Hızlı başlangıç: yüz REST API ve C 'yi kullanarak görüntüdeki yüzeyleri algılama #

Bu hızlı başlangıçta, görüntüde insan yüzlerini saptamak Için Azure yüz REST API C# ile kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Yüz Tanıma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
- Herhangi bir [Visual Studio](https://www.visualstudio.com/downloads/)sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio 'da yeni bir **konsol uygulaması (.NET Framework)** projesi oluşturun ve bunu **Facedetection**olarak adlandırın.
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.

## <a name="add-face-detection-code"></a>Yüz algılama kodu ekleme

Yeni projenin *program.cs* dosyasını açın. Burada, görüntüleri yüklemek ve yüzeyleri algılamak için gereken kodu ekleyeceksiniz.

### <a name="include-namespaces"></a>Ad alanlarını ekleme

Aşağıdaki `using` deyimlerini *Program.cs* dosyanızın en üstüne ekleyin.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="dependencies":::

### <a name="add-essential-fields"></a>Gerekli alanları Ekle

Aşağıdaki alanları içeren **Program** sınıfını ekleyin. Bu veriler, yüz hizmetine bağlanmayı ve giriş verilerinin nereden alınacağını belirtir. `subscriptionKey`Alanı abonelik anahtarınızın değeriyle güncelleştirmeniz gerekir ve bu `uriBase` dizeyi, kaynak uç nokta dizenizi içerecek şekilde değiştirmeniz gerekebilir.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="environment":::

### <a name="receive-image-input"></a>Görüntü girişi al

**Program** sınıfının **Main** yöntemine aşağıdaki kodu ekleyin. Bu kod, kullanıcıdan yerel görüntü dosya yolunu girmesini isteyen konsola bir istem yazar. Daha sonra bu konumdaki görüntüyü işlemek için, **Makeanalysisrequest**, başka bir yöntemi çağırır.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="main":::

### <a name="call-the-face-detection-rest-api"></a>Yüz algılamayı çağırın REST API

**Program** sınıfına aşağıdaki yöntemi ekleyin. Uzak görüntüdeki yüz bilgilerini algılamak için Yüz Tanıma API'si bir REST çağrısı oluşturur ( `requestParameters` dize hangi yüz özniteliklerini almak üzere belirtir). Ardından, çıkış verilerini bir JSON dizesine yazar.

Aşağıdaki adımlarda yardımcı yöntemleri tanımlayacaksınız.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="request":::

### <a name="process-the-input-image-data"></a>Giriş resim verilerini işleme

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, belirtilen dosya yolundaki görüntüyü bir bayt dizisine dönüştürür.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="getimage":::

### <a name="parse-the-json-response"></a>JSON yanıtını Ayrıştır

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem JSON girişini daha kolay okunabilir olacak şekilde biçimlendirir. Uygulamanız, bu dize verilerini konsola yazacak. Daha sonra sınıfı ve ad alanını kapatabilirsiniz.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="print":::

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Başarılı bir yanıt, yüz verileri kolay okunabilir JSON biçiminde görüntüler. Örnek:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      }
   }
]
```

## <a name="extract-face-attributes"></a>Yüz özniteliklerini Ayıkla
 
Yüz özniteliklerini ayıklamak için, algılama modeli 1 ' i kullanın ve `returnFaceAttributes` sorgu parametresini ekleyin.

```csharp
string requestParameters = "detectionModel=detection_01&returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

Yanıt şimdi yüz öznitelikleri içeriyor. Örnek:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      },
      "faceAttributes": {
         "smile": 0.0,
         "headPose": {
            "pitch": 0.0,
            "roll": 0.1,
            "yaw": -32.9
         },
         "gender": "female",
         "age": 22.9,
         "facialHair": {
            "moustache": 0.0,
            "beard": 0.0,
            "sideburns": 0.0
         },
         "glasses": "NoGlasses",
         "emotion": {
            "anger": 0.0,
            "contempt": 0.0,
            "disgust": 0.0,
            "fear": 0.0,
            "happiness": 0.0,
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
            "value": 0.0
         },
         "makeup": {
            "eyeMakeup": true,
            "lipMakeup": true
         },
         "accessories": [

         ],
         "occlusion": {
            "foreheadOccluded": false,
            "eyeOccluded": false,
            "mouthOccluded": false
         },
         "hair": {
            "bald": 0.0,
            "invisible": false,
            "hairColor": [
               {
                  "color": "brown",
                  "confidence": 1.0
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

Bu hızlı başlangıçta, bir görüntüdeki yüzeyleri tespit etmek ve özniteliklerini döndürmek için Azure yüz hizmeti ile REST çağrılarını kullanan basit bir .NET konsol uygulaması oluşturdunuz. Daha sonra, desteklenen senaryolar hakkında daha fazla bilgi edinmek için Yüz Tanıma API'si başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
