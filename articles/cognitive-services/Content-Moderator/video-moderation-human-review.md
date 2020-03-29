---
title: İnsan incelemesi ile Video ılımlılığı - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Uygun olmayan içeriği ortalamak için makine destekli video moderasyonu ve insan inceleme araçlarını kullanma
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: pafarley
ms.openlocfilehash: a4e7b079367a4b4dec1d2b3d6c0afde1d8276766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72754165"
---
# <a name="video-moderation-with-human-review"></a>İnsan incelemesi ile video ılımlılığı

İçeriğiniz için en iyi sonuçları almak için yetişkinlere uygun (müstehcen) ve müstehcen (müstehcen) içerik için videoları ve transkriptleri ortalamak için İçerik Moderatör'ün makine destekli [video moderasyonunu](video-moderation-api.md) ve [insan inceleme aracını](Review-Tool-User-Guide/human-in-the-loop.md) kullanın.

## <a name="video-trained-classifier-preview"></a>Video eğitimli sınıflandırıcı (önizleme)

Makine destekli video sınıflandırması ya görüntü eğitimli modeller veya video eğitimli modeller ile elde edilir. Görüntü eğitimi almış video sınıflandırıcılarının aksine, Microsoft'un yetişkinlere yönelik ve müstehcen video sınıflandırıcısı videolarla eğitilir. Bu yöntem daha iyi maç kalitesi sağlar.

## <a name="shot-detection"></a>Atış algılama

Sınıflandırma ayrıntılarını çıkarırken, ek video zekası videoları analiz etmede daha fazla esneklik sağlar. Microsoft'un video denetleme hizmeti, yalnızca kareleri çıkarmak yerine, çekim düzeyi bilgileri de sağlar. Artık videolarınızı çekim düzeyinde ve kare düzeyinde analiz etme seçeneğiniz vardır.

## <a name="key-frame-detection"></a>Anahtar kare algılama

Video moderasyon hizmeti, kareleri düzenli aralıklarla çıkmak yerine, yalnızca tamamlanmış (iyi) kareleri tanımlar ve çıktıları belirler. Bu özellik, çerçeve düzeyinde yetişkin ve müstehcen analizler için verimli çerçeve oluşturma olanağı sağlar.

Aşağıdaki özü potansiyel çekim, anahtar kareler ve yetişkin ve müstehcen puanları ile kısmi bir yanıt gösterir:

```json
"fragments":[  
  {  
    "start":0,
    "duration":18000
  },
  {  
    "start":18000,
    "duration":3600,
    "interval":3600,
    "events":[  
      [  
        {  
          "reviewRecommended":false,
          "adultScore":0.00001,
          "racyScore":0.03077,
          "index":5,
          "timestamp":18000,
          "shotIndex":0
        }
      ]
    ]
  },
  {  
    "start":18386372,
    "duration":119149,
    "interval":119149,
    "events":[  
      [  
        {  
          "reviewRecommended":true,
          "adultScore":0.00000,
          "racyScore":0.91902,
          "index":5085,
          "timestamp":18386372,
          "shotIndex":62
        }
      ]
    ]
```

## <a name="visualization-for-human-reviews"></a>İnsan değerlendirmeleri için görselleştirme

Daha incelikli durumlar için, işletmelerin videoyu, çerçevelerini ve makinetarafından atanmış etiketleri işlemek için bir insan inceleme çözümüne ihtiyacı vardır. Videoları ve kareleri inceleyen insan moderatörler, öngörülerin tam bir görünümünü alır, etiketleri değiştirir ve kararlarını gönderir.

![video inceleme aracı varsayılan görünümü](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Video düzeyinde inceleme için oyuncu görünümü

Video düzeyinde ikili kararlar, potansiyel yetişkin ve müstehcen kareleri gösteren bir video oynatıcı görünümü yle mümkün kılınabilir. İnsan yorumcular sahneleri incelemek için çeşitli hız seçenekleri ile video gezinmek. Etiketleri değiştirerek kararlarını onaylıyorlar.

![video inceleme aracı oynatıcı görünümü](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Ayrıntılı incelemeler için çerçeve görünümü

Kare kare çözümleme için ayrıntılı bir video incelemesi kare tabanlı bir görünümle mümkün kılınz. İnsan gözden geçirenler, kararlarını onaylamak için bir veya daha fazla kareyi ve etiketleri gözden geçirip seçer. İsteğe bağlı bir sonraki adım, saldırgan çerçevelerin veya içeriğin redaksiyonudur.

![video inceleme araç çerçeveleri görünümü](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Transkripti denetleme

Videolar genellikle rahatsız edici konuşma için de ılımlılık ihtiyacı üzerinde ses var. Konuşmayı metne dönüştürmek ve inceleme aracı içindeki metin moderasyonu için transkriptgöndermek için İçerik Moderatörü'nün inceleme API'sini kullanmak için Azure Media Indexer hizmetini kullanırsınız.

![video inceleme aracı transkript görünümü](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Video ılımlılığı quickstart](video-moderation-api.md)ile başlayın.
- Denetlenmiş çıktınızdan insan gözden geçirenleriniz için [nasıl video incelemeleri](video-reviews-quickstart-dotnet.md) oluşturacağınızı öğrenin.
- Video [incelemelerinize video transkript yorumları](video-transcript-reviews-quickstart-dotnet.md) ekleyin.
- Tam bir [video ılımlılık çözümü](video-transcript-moderation-review-tutorial-dotnet.md)geliştirmek için nasıl ayrıntılı öğretici göz atın.