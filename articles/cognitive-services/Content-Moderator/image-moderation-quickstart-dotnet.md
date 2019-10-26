---
title: 'Hızlı başlangıç: Content Moderator sakıncalı içerik C# Için görüntüleri analiz etme'
titleSuffix: Azure Cognitive Services
description: .NET için Content Moderator SDK kullanarak çeşitli sakıncalı malzemeler için görüntü içeriğini çözümleme
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: pafarley
ms.openlocfilehash: 13b0952f38fb0c8c922be415f782b3a0a0861729
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931746"
---
# <a name="quickstart-analyze-images-for-objectionable-content-in-c"></a>Hızlı başlangıç: ' de sakıncalı içerik için görüntüleri analiz etmeC#

Bu makalede, [.NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için Content Moderator SDK'sını kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanır. Yetişkinlere yönelik içeriği, ayıklanabilir metnini ve insan yüzlerini nasıl tarayacağını potansiyel olarak sakıncalı bir malzemeden nasıl tarayabileceğinizi öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Content Moderator abonelik anahtarı. Content Moderator abone olmak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Ardından, sırasıyla `CONTENT_MODERATOR_SUBSCRIPTION_KEY` ve `CONTENT_MODERATOR_ENDPOINT`adlı anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü


> [!NOTE]
> Bu kılavuzda ücretsiz katmanı Content Moderator aboneliği kullanılır. Her abonelik katmanında nelerin sağlandığı hakkında bilgi için [Fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/) sayfasına bakın.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun ve projeyi **ImageModeration** olarak adlandırın. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin; ardından aşağıdaki projeleri bulun ve yükleyin:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Görüntü denetimi kodu ekleme

Ardından, temel bir içerik moderasyonu senaryosu uygulamak için kodu bu kılavuzdan kopyalayıp projenize yapıştıracaksınız.

### <a name="include-namespaces"></a>Ad alanlarını ekleme

Aşağıdaki `using` deyimlerini *Program.cs* dosyanızın en üstüne ekleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_using)]

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğinize bir Content Moderator istemci sağlayıcısı oluşturmak için aşağıdaki kodu *Program.cs* dosyanıza ekleyin. Aynı ad alanında **Program** sınıfının yanına sınıfı ekleyin. **AzureBaseURL** ve **cmsubscriptionkey** alanlarını Endpoint URL 'nizin ve abonelik anahtarınızın değerleriyle güncelleştirmeniz gerekir. Bunları, Azure portal kaynağınızın **hızlı başlangıç** sekmesinde bulabilirsiniz.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_client)]


### <a name="set-up-input-and-output-targets"></a>Giriş ve çıkış hedeflerini ayarlama

Aşağıdaki statik alanları _Program.cs_ dosyasındaki **Program** sınıfına ekleyin. Bu alanlar, giriş görüntüsü içeriği ve çıkış JSON içeriği için dosyaları belirtir.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_fields)]

*Imagefiles. txt* giriş dosyasını oluşturmanız ve yolunu uygun şekilde güncelleştirmeniz gerekir (göreli yollar yürütme dizinine görelidir). _ImageFiles.txt_ dosyasını açın ve denetlenecek görüntülerin URL'lerini ekleyin. Bu hızlı başlangıçta örnek giriş olarak aşağıdaki URL'ler kullanılır.

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Sonuçları işlemek için bir sınıf oluşturma

Aynı ad alanında *Program* sınıfıyla birlikte **Program.cs** dosyasına aşağıdaki kodu ekleyin. Gözden geçirilen görüntülerin her birinin denetim sonuçlarını kaydetmek için bu sınıfın bir örneğini kullanacaksınız.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_dataclass)]


### <a name="define-the-image-evaluation-method"></a>Görüntü değerlendirme yöntemini tanımlama

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, tek bir görüntüyü üç farklı yoldan değerlendirir ve değerlendirme sonuçlarını döndürür. Bu tek tek işlemlerin ne yaptığı hakkında daha fazla bilgi edinmek istiyorsanız, [Sonraki adımlar](#next-steps) bölümündeki bağlantıyı izleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_evaluate)]

### <a name="load-the-input-images"></a>Giriş görüntülerini yükleme

Aşağıdaki kodu **Program** sınıfındaki **Main** yöntemine ekleyin. Bu kod, giriş dosyasındaki her bir görüntü URL 'SI için değerlendirme verilerini almak üzere programı ayarlar. Ardından bu verileri tek bir çıkış dosyasına yazar.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?name=snippet_main)]

## <a name="run-the-program"></a>Programı çalıştırma

Program JSON dize verilerini _ModerationOutput.json_ dosyasına yazacaktır. Bu hızlı başlangıçta kullanılan örnek görüntüler aşağıdaki çıkışı verir. Her görüntüde, **EvaluateImage** YÖNTEMINIZIN üç API çağrısına karşılık gelen `ImageModeration`, `FaceDetection` ve `TextDetection` için farklı bölümler bulunur.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Content Moderator hizmetini kullanarak belirli bir görüntü örneği hakkındaki ilgili bilgileri döndüren basit bir .NET uygulaması geliştirdiniz. Bundan sonra, hangi verilere ihtiyacınız olduğuna ve uygulamanızın bunları nasıl işleyeceğine karar verebilmek için farklı bayrakların ve sınıflandırmaların anlamları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Görüntü moderasyonu kılavuzu](image-moderation-api.md)
