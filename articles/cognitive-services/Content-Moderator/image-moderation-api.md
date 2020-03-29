---
title: Görüntü Moderasyonu - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Yetişkinlere uygun ve müstehcen içerik için görüntüleri ılımlı yapmak için İçerik Moderatör'ün makine destekli görüntü moderasyonu ve döngü içinde insan İnceleme aracını kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 6d419135a1da68d23689f66d3134a638d71a3eab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73044110"
---
# <a name="learn-image-moderation-concepts"></a>Görüntü moderasyon kavramlarını öğrenin

Yetişkinlere uygun ve müstehcen içerik için görüntüleri ılımlı yapmak için İçerik Moderatör'ün makine destekli görüntü moderasyonu ve [döngü içinde insan İnceleme aracını](Review-Tool-User-Guide/human-in-the-loop.md) kullanın. Metin içeriği için görüntüleri tarayıp bu metni ayıklayın ve yüzleri algılayın. Görüntüleri özel listelerle eşleyebilir ve daha fazla işlem yapabilirsiniz.

## <a name="evaluating-for-adult-and-racy-content"></a>Yetişkinlere uygun ve müstehcen içerik için değerlendirme

**Değerlendirme** işlemi 0 ile 1 arasında bir güven puanı döndürür. Ayrıca boolean verileri doğru veya yanlış eşit döndürür. Bu değerler, görüntünün olası yetişkinlere uygun veya müstehcen içerik içerip içerme API'yi resminizle (dosya veya URL) aradiğinizde, döndürülen yanıt aşağıdaki bilgileri içerir:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]
> 
> - `isImageAdultClassified`, belirli durumlarda açık saçık veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
> - `isImageRacyClassified`, belirli durumlarda davetkar veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
> - Puanlar 0 ile 1 arasındadır. Puan ne kadar yüksekse, model kategorinin uygulanabilir olabileceğini tahmin ediyor. Bu önizleme, el ile kodlanmış sonuçlar yerine istatistiksel bir modele dayanır. Her kategorinin gereksinimlerinize nasıl uygun olduğunu belirlemek için kendi içeriğinizle test yapmanızı öneririz.
> - Boolean değerleri iç puan eşiklerine bağlı olarak doğru veya yanlıştır. Müşteriler bu değeri kullanıp kullanmayacağını veya içerik ilkelerine göre özel eşiklere karar verip vermeyeceğine karar vermelidir.

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR) ile metni algılama

**Optik Karakter Tanıma (OCR)** işlemi, görüntüdeki metin içeriğinin varlığını tahmin eder ve diğer kullanımların yanı sıra metin moderasyonu için ayıklar. Dili belirtebilirsiniz. Bir dil belirtmezseniz, algılama varsayılan Olarak İngilizce'ye gelir.

Yanıt aşağıdaki bilgileri içerir:
- Orijinal metin.
- Güven puanları ile algılanan metin öğeleri.

Örnek ekstre:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Yüz algılama

Yüzleri algılamak, görüntülerdeki yüzler gibi kişisel verilerin algılanmasına yardımcı olur. Olası yüzleri ve her görüntüdeki potansiyel yüz sayısını algılarsınız.

Yanıt şu bilgileri içerir:

- Yüzler sayılır
- Algılanan yüzlerin konumları listesi

Örnek ekstre:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
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
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Özel listeler oluşturma ve yönetme

Birçok çevrimiçi toplulukta, kullanıcılar resim veya başka tür içerik yükledikten sonra, rahatsız edici öğeler sonraki gün, hafta ve aylarda birden çok kez paylaşılabilir. Aynı görüntüyü ve hatta görüntünün birden çok yerden biraz değiştirilmiş sürümlerini tekrar tekrar tarama nın ve filtrelemenin maliyeti pahalı ve hataya açık olabilir.

Aynı görüntüyü birden çok kez denetlemek yerine, rahatsız edici görüntüleri engellenen özel içeriğiniz listenize eklersiniz. Bu şekilde, içerik denetleme sisteminiz gelen görüntüleri özel listelerinizle karşılaştırır ve başka işlemleri durdurur.

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

İçerik Moderatör özel görüntülerin listelerini yönetmek için işlemleri ile tam bir [Görüntü Listesi Yönetimi API](try-image-list-api.md) sağlar. [Resim Listeleri API Konsolu](try-image-list-api.md) ile başlayın ve REST API kod örneklerini kullanın. Visual Studio ve C#'a aşinaysanız [Resim Listesi .NET quickstart'a](image-lists-quickstart-dotnet.md) da göz atın.

## <a name="matching-against-your-custom-lists"></a>Özel listelerinize göre eşleştirme

Eşleç işlemi, Liste işlemlerini kullanarak oluşturulan ve yönetilen özel listelerinizden herhangi biri ile gelen görüntülerin bulanık bir şekilde eşleştirilmesine olanak tanır.

Bir eşleşme bulunursa, işlem eşleşen görüntünün tanımlayıcısını ve Denetleme etiketlerini döndürür. Yanıt şu bilgileri içerir:

- Maç skoru (0 ile 1 arasında)
- Eşleşen görüntü
- Resim etiketleri (önceki ılımlılık sırasında atanmış)
- Resim etiketleri

Örnek ekstre:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>İnsan inceleme aracı

Daha incelikli durumlar için, insan moderatörleriniz için incelemedeki ılımlılık sonuçlarını ve içeriğini ortaya çıkarmak için İçerik [Moderatörü inceleme aracını](Review-Tool-User-Guide/human-in-the-loop.md) ve API'sini kullanın. Makinetarafından atanan etiketleri inceler ve nihai kararlarını onaylarlar.

![İnsan denetimciler için görüntü incelemesi](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Sonraki adımlar

[Görüntü Moderasyonu API konsolunu](try-image-api.md) test edin ve REST API kod örneklerini kullanın. Visual Studio ve C#'a aşinaysanız [.NET SDK quickstart'ın](dotnet-sdk-quickstart.md) Görüntü Moderasyon bölümüne de göz atın.
