---
title: Video Indexer-Azure ile konuşulan dili otomatik olarak tanımla
titlesuffix: Azure Media Services
description: Bu makalede, bir videodaki konuşulan dili otomatik olarak tanımlamak için Video Indexer dil tanımlama modelinin nasıl kullanıldığı açıklanmaktadır.
services: media-services
author: juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/22/2019
ms.author: ellbe
ms.openlocfilehash: ea7d2c451127283f2af204de2c961747880e1bfe
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494896"
---
# <a name="automatically-identify-the-spoken-language-with-language-identification-model"></a>Dil tanımlama modeliyle konuşulan dili otomatik olarak tanımla

Video Indexer, otomatik dil kimliğini (kapak) destekler, bu, sesli okunan dil içeriğini otomatik olarak tanımlama ve baskın tanımlanmış dilde kullanılacak medya dosyasını gönderme işlemidir. Şu anda KAPAĞı Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Portekizce (Brezilya) desteklemektedir. 

## <a name="choosing-auto-language-identification-on-indexing"></a>Dizin oluşturma sırasında otomatik dil tanımlamayı seçme

API 'yi kullanarak bir videoyu dizinleme veya `sourceLanguage` [yeniden dizinlerken](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) , parametresindeki `auto detect` seçeneği belirleyin.

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

## <a name="guidelines-and-limitations"></a>Kılavuzlar ve sınırlamalar

* Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Brezilya Portekizcesi.
* Ses yukarıda desteklenen listeden farklı diller içeriyorsa sonuç beklenmedik olur.
* Video Indexer, yüksek oranda güvenilirlikli (`>0.6`) dili tanımlayamıyor, geri dönüş dili İngilizce olur.
* Karma diller ses içeren dosya için geçerli destek yoktur. Ses karışık diller içeriyorsa, sonuç beklenmedik olur. 
* Düşük kaliteli ses, model sonuçlarını etkileyebilir.
* Model, sese en az bir dakikalık konuşma gerektirir.
* Model, Spontaneous bir konuşmayı tanımak için tasarlanmıştır (ses komutları, Malama, vb.).

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
