---
title: Konuşulan dilleri otomatik olarak tanımlamak için Video Indexer kullanın-Azure
titleSuffix: Azure Media Services
description: Bu makalede, bir videodaki konuşulan dili otomatik olarak tanımlamak için Video Indexer dil tanımlama modelinin nasıl kullanıldığı açıklanmaktadır.
services: media-services
author: juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/12/2020
ms.author: ellbe
ms.openlocfilehash: 3a71a29fdf4af10162e2f7961fb457d0e99b18e8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687132"
---
# <a name="automatically-identify-the-spoken-language-with-language-identification-model"></a>Dil tanımlama modeliyle konuşulan dili otomatik olarak tanımla

Video Indexer, otomatik dil kimliğini (kapak) destekler, bu, sesli okunan dil içeriğini otomatik olarak tanımlama ve baskın tanımlanmış dilde kullanılacak medya dosyasını gönderme işlemidir. 

Şu anda KAPAĞı şunları destekler: Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Mandarin Çince, Japonca, Rusça ve Portekizce (Brezilya). 

Aşağıdaki [kılavuz ve sınırlamalar](#guidelines-and-limitations) bölümünü gözden geçirdiğinizden emin olun.

## <a name="choosing-auto-language-identification-on-indexing"></a>Dizin oluşturma sırasında otomatik dil tanımlamayı seçme

API 'yi kullanarak bir videoyu dizinleme veya [yeniden dizinlerken](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) , `auto detect` `sourceLanguage` parametresindeki seçeneği belirleyin.

Portalı kullanırken [video Indexer](https://www.videoindexer.ai/) giriş sayfasında **Hesap videolarınıza** gidin ve yeniden dizinlemek istediğiniz videonun adının üzerine gelin. Sağ alt köşede yeniden dizin düğmesine tıklayın. **Yeniden dizin videosu** iletişim kutusunda, **video kaynak dili** açılır listesinden *Otomatik Algıla* ' yı seçin.

![Otomatik Algıla](./media/language-identification-model/auto-detect.png)

## <a name="model-output"></a>Model çıkışı

Video Indexer söz konusu dilin güvenilirliği ise, `> 0.6`videoyu en olası dile göre. Dil güvenle tanımlanamıyorsa, konuşulan dilin Ingilizce olduğunu varsayar. 

Model baskın dil, Öngörüler JSON 'da `sourceLanguage` öznitelik olarak kullanılabilir (kök/videolar/Öngörüler altında). Karşılık gelen bir güven puanı `sourceLanguageConfidence` özniteliği altında da kullanılabilir.

```json
"insights": {
        "version": "1.0.0.0",
        "duration": "0:05:30.902",
        "sourceLanguage": "fr-FR",
        "language": "fr-FR",
        "transcript": [...],
        . . .
        "sourceLanguageConfidence": 0.8563
      },
```

## <a name="guidelines-and-limitations"></a>Yönergeler ve sınırlamalar

* Otomatik dil tanımlama (kapak) aşağıdaki dilleri destekler: 

    İngilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Mandarin, Japonca, Rusça ve Portekizce (Brezilya).
* Video Indexer, Arapça (modern standart ve Levantine), Hintçe ve Korece 'yi desteklese de bu diller KAPAPLANDA desteklenmez.
* Ses yukarıda desteklenen listeden farklı diller içeriyorsa sonuç beklenmedik olur.
* Video Indexer, yüksek oranda güvenilirlikli (`>0.6`) dili tanımlayamıyor, geri dönüş dili İngilizce olur.
* Karma diller ses içeren dosya için geçerli destek yoktur. Ses karışık diller içeriyorsa, sonuç beklenmedik olur. 
* Düşük kaliteli ses, model sonuçlarını etkileyebilir.
* Model, sese en az bir dakikalık konuşma gerektirir.
* Model, Spontaneous bir konuşmayı tanımak için tasarlanmıştır (ses komutları, Malama, vb.).

## <a name="next-steps"></a>Sonraki adımlar

* [Genel bakış](video-indexer-overview.md)
* [Çoklu dil içeriğini otomatik olarak tanımla ve yeniden tanımla](multi-language-identification-transcription.md)
