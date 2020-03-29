---
title: REST API konsolu ile ılımlılık değerlendirmeleri oluşturun - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: İnsan Moderasyonu için görüntü veya metin incelemeleri oluşturmak için Azure İçerik Moderatör İnceleme API'lerini kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: pafarley
ms.openlocfilehash: a9726e41a84926d00d48b51e31f534a3d8c2fe0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72757133"
---
# <a name="create-human-reviews-rest"></a>İnsan yorumları oluşturun (REST)

[İncelemeler,](./review-api.md#reviews) insan moderatörlerin değerlendirmek için içeriği saklar ve görüntüler. Bir kullanıcı incelemeyi tamamladığında, sonuçlar belirtilen geri arama bitiş noktasına gönderilir. Bu kılavuzda, API konsolu aracılığıyla derleme REST API'lerini kullanarak incelemeleri nasıl ayarlayacağımı öğreneceksiniz. API'lerin yapısını anladıktan sonra, bu çağrıları REST uyumlu herhangi bir platforma kolayca iletebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.

## <a name="create-a-review"></a>İnceleme oluşturma

İnceleme oluşturmak için Gözden **[Geçirme -](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** API başvuru sayfası oluşturun ve anahtar bölgeniz için düğmeyi seçin (bunu [Gözden Geçirme aracının](https://contentmoderator.cognitive.microsoft.com/) **Kimlik Bilgileri** sayfasında Ki Bitiş Noktası URL'sinde bulabilirsiniz). Bu, REST API çağrılarını kolayca oluşturabileceğiniz ve çalıştırabileceğiniz API konsolu'nu başlatır.

![İnceleme - Bölge seçimini alın](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST çağrı parametrelerini girin

**TeamName**ve **Ocp-Apim-Abonelik-Anahtar**için değerleri girin:

- **teamName**: [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabınızı ayarlarken oluşturduğunuz takım kimliği (İnceleme aracınızın Kimlik Bilgileri ekranında **Kimlik** alanında bulunur).
- **Ocp-Apim-Abonelik-Anahtar**: İçerik Moderatör anahtarınız. Bunu [Gözden Geçir aracının](https://contentmoderator.cognitive.microsoft.com) **Ayarlar** sekmesinde bulabilirsiniz.

### <a name="enter-a-review-definition"></a>İnceleme tanımı girin

JSON isteğini aşağıdaki alanları içeren girmek için **İstek gövde** kutusunu edin:

- **Meta veriler**: Geri arama bitiş noktanıza döndürülecek özel anahtar değeri çiftleri. Anahtar [Gözden Geçirme aracında](https://contentmoderator.cognitive.microsoft.com)tanımlanan kısa bir kodsa, etiket olarak görünür.
- **İçerik**: Resim ve Video içeriği söz konusu olduğunda, bu içeriğe işaret eden bir URL dizesidir. Metin içeriği için bu gerçek metin dizesidir.
- **ContentId**: Özel tanımlayıcı dize. Bu dize API'ye aktarılır ve geri arama yoluyla döndürülür. İç tanımlayıcıları veya meta verileri ılımlılık işinin sonuçlarıyla ilişkilendirmede yararlıdır.
- **CallbackEndpoint**: (İsteğe bağlı) İnceleme tamamlandığında geri arama bilgilerini almak için URL.

Varsayılan istek gövdesi, oluşturabileceğiniz farklı inceleme türlerinin örneklerini gösterir:

```json
[Image]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Image",
    "Content": "<Content Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Text]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Text",
    "Content": "<Your Text Content>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Video]
[
  {
    "VideoFrames":[
      {
          "Id": "<Frame Id>",
          "Timestamp": "<Frame Timestamp",
          "FrameImage":"<Frame Image URL",
          "Metadata": [
            {
              "Key": "<Key>",
              "Value": "<Value"
            }
          ],
          "ReviewerResultTags": [
          ]
    ], 
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      },
      //For encrypted Videos
        {
          "Key": "protectedType",
          "Value": "AES or FairPlay or Widevine or Playready"
        },
        {
          "Key": "authenticationToken",
          "Value": "your viewtoken(In case of Video Indexer AES encryption type, this value is viewtoken from breakdown json)"
        },
      //For FairPlay encrypted type video include certificateUrl as well
        {
          "Key": "certificateUrl",
          "Value": "your certificate url"
        }
    ],
    "Type": "Video",
    "Content": "<Stream Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>",
    [Optional]
    "Timescale": "<Timescale of the video>
  }
]
```

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içerik** kutusu gözden geçirme için bir kimlik görüntüler. Aşağıdaki adımlarda kullanmak üzere bu kimliği kopyalayın.

![İnceleme - Konsol Oluştur Yanıt içerik kutusu gözden geçirme kimliğini görüntüler](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Yeni incelemeyi inceleyin

Gözden [Geçir aracında,](https://contentmoderator.cognitive.microsoft.com)**Resim**/**Metni**/**Videosunu** İncele (kullandığınız içeriğe bağlı olarak) **seçin.** >  Yüklediğiniz içerik, insan incelemesi için hazır görünmelidir.

![Bir futbol topunun araç görüntüsünü gözden geçirme](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>İnceleme ayrıntılarını alın

Varolan bir incelemeyle ilgili ayrıntıları almak için [Gözden Geçirme -](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) API başvuru sayfasına gidin ve bölgeniz (anahtarınızın yönetildiği bölge) düğmesini seçin.

![İş akışı - Bölge seçimini alın](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST arama parametrelerini girin. Bu adım için **reviewId,** incelemeyi oluşturduğunuzda aldığınız benzersiz kimlik dizesidir.

![İnceleme - Konsol oluştur Sonuçları alın](images/test-drive-review-3.PNG)
  
**Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içerik** kutusu inceleme ayrıntılarını JSON biçiminde aşağıdaki gibi görüntüler:

```json
{  
  "reviewId":"201712i46950138c61a4740b118a43cac33f434",
  "subTeam":"public",
  "status":"Complete",
  "reviewerResultTags":[  
    {  
      "key":"a",
      "value":"False"
    },
    {  
      "key":"r",
      "value":"True"
    },
    {  
      "key":"sc",
      "value":"True"
    }
  ],
  "createdBy":"<teamname>",
  "metadata":[  
    {  
      "key":"sc",
      "value":"true"
    }
  ],
  "type":"Image",
  "content":"https://reviewcontentprod.blob.core.windows.net/<teamname>/IMG_201712i46950138c61a4740b118a43cac33f434",
  "contentId":"0",
  "callbackEndpoint":"<callbackUrl>"
}
```

Yanıtta aşağıdaki alanları dikkate alın:

- **durum**
- **reviewerResultTags**: İnsan inceleme ekibi tarafından el ile herhangi bir etiket eklenmiştirsa **(oluşturulanBy** alanı gösterilir).
- **meta veriler**: İnsan inceleme ekibi değişiklik yapmadan önce, incelemede başlangıçta eklenen etiketleri gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, REST API'sini kullanarak içerik Denetleme yorumlarının nasıl oluşturulabileceğinizi öğrendiniz. Ardından, yorumları [E-ticaret Denetleme](./ecommerce-retail-catalog-moderation.md) Öğreticisi gibi uçdan uca bir Denetleme senaryosuna entegre edin.