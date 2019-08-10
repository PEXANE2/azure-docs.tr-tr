---
title: REST API konsolu ile denetleme incelemeleri oluşturma-Content Moderator
titleSuffix: Azure Cognitive Services
description: İnsan denetlemesi için görüntü veya metin incelemesi oluşturmak üzere Azure Content Moderator gözden geçirme API 'Lerini kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: ec45f182e24f44c2222d64f18e2aa0aeea845727
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882339"
---
# <a name="create-human-reviews-rest"></a>İnsan incelemeleri oluşturma (REST)

, İnsan moderatör 'nin değerlendirilmesine yönelik depolamayı ve görüntüleme içeriğini [gözden geçirir](./review-api.md#reviews) . Bir Kullanıcı bir gözden geçirmeyi tamamladığında, sonuçlar belirtilen geri çağırma uç noktasına gönderilir. Bu kılavuzda, API konsolundan REST API 'Lerini gözden geçirme ' yi kullanarak İncelemeleri ayarlamayı öğreneceksiniz. API 'lerin yapısını anladıktan sonra, bu çağrıları REST ile uyumlu herhangi bir platforma kolayca bağlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Content Moderator [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya hesap oluşturun.

## <a name="create-a-review"></a>İnceleme oluştur

Bir gözden geçirme oluşturmak için, **[Gözden geçirme-](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** API başvurusu oluşturma sayfasına gidin ve anahtar bölgeniz için düğmeyi seçin (bunu [Inceleme aracının](https://contentmoderator.cognitive.microsoft.com/) **KIMLIK bilgileri** sayfasında uç nokta URL 'sinde bulabilirsiniz). Bu, REST API çağrılarını kolayca oluşturabileceğiniz ve çalıştırabileceğiniz API konsolunu başlatır.

![İnceleme-bölge seçimini al](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST çağrı parametrelerini girin

**TeamName**ve **OCP-apim-Subscription-Key**değerlerini girin:

- **ekip adı**: [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabınızı ayarlarken oluşturduğunuz takım kimliği (gözden geçirme aracınızın kimlik bilgileri ekranındaki **kimlik** alanında bulunur).
- **OCP-apim-Subscription-Key**: Content Moderator anahtarınız. Bunu [İnceleme aracının](https://contentmoderator.cognitive.microsoft.com) **Ayarlar** sekmesinde bulabilirsiniz.

### <a name="enter-a-review-definition"></a>Bir gözden geçirme tanımı girin

JSON isteğini aşağıdaki alanlarla girmek için **İstek gövdesi** kutusunu düzenleyin:

- **Meta veriler**: Geri çağırma uç noktanıza döndürülecek özel anahtar-değer çiftleri. Anahtar [İnceleme aracında](https://contentmoderator.cognitive.microsoft.com)tanımlanan kısa bir koddur, etiket olarak görünür.
- **İçerik**: Görüntü ve video içeriği söz konusu olduğunda, içeriği gösteren bir URL dizesidir. Metin içeriği için bu gerçek metin dizesidir.
- **ContentID**: Özel bir tanımlayıcı dizesi. Bu dize, API 'ye geçirilir ve geri çağırma yoluyla döndürülür. Bir denetleme işinin sonuçlarıyla iç tanımlayıcıları veya meta verileri ilişkilendirmek için faydalıdır.
- **Callbackendpoint**: Seçim İnceleme tamamlandığında geri çağırma bilgilerini alacak URL.

Varsayılan istek gövdesi, oluşturabileceğiniz farklı gözden geçirmeler türlerini gösterir:

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

### <a name="submit-your-request"></a>İsteğinizi gönderme
  
**Gönder**’i seçin. İşlem başarılı olursa, **yanıt durumu** olur `200 OK`ve **yanıt içeriği** kutusu İnceleme için bir kimlik görüntüler. Aşağıdaki adımlarda kullanmak için bu KIMLIĞI kopyalayın.

![İnceleme-konsol yanıt içeriği oluştur kutusu İnceleme KIMLIĞINI görüntüler](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Yeni incelemeyi inceleyin

[İnceleme aracında](https://contentmoderator.cognitive.microsoft.com) > **görüntü** **metni videosunu gözden**geçir ' i (kullandığınız içeriğe göre) seçin.// Karşıya yüklediğiniz içerik görünür, insan gözden geçirmesi için hazırlanın.

![Futbol topu araç görüntüsünü gözden geçirme](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>İnceleme ayrıntılarını al

Mevcut bir gözden geçirme hakkındaki ayrıntıları almak için [İnceleme-](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) API başvurusunu Al sayfasına gidin ve bölgeniz için (anahtarınızın yönettiği bölge) düğmesini seçin.

![İş akışı-bölge seçimini al](images/test-drive-region.png)

REST çağrı parametrelerini yukarıdaki bölümde olduğu gibi girin. Bu adım için, **Revieıd** , gözden geçirmeyi oluştururken ALDıĞıNıZ benzersiz kimlik dizesidir.

![İnceleme-konsol oluşturma sonuçları al](images/test-drive-review-3.PNG)
  
**Gönder**’i seçin. İşlem başarılı olursa, **yanıt durumu** olur `200 OK`ve **yanıt içeriği** kutusu, gözden geçirme ayrıntılarını aşağıdakiler gibi JSON biçiminde görüntüler:

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

Yanıtta aşağıdaki alanlara göz atın:

- **status**
- **Reviewerresulttags**: Bu, insan gözden geçirme ekibi tarafından el ile eklenen herhangi bir etiket varsa görünür ( **CreatedBy** alanı gösteriliyor).
- **meta veriler**: Bu, insan gözden geçirme ekibi değişiklik yapmadan önce incelemeye eklenen etiketleri gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, REST API kullanarak içerik denetleme İncelemeleri oluşturmayı öğrendiniz. Ardından, İncelemeleri [E-ticaret denetleme](./ecommerce-retail-catalog-moderation.md) öğreticisi gibi uçtan uca bir denetleme senaryosuna tümleştirin.