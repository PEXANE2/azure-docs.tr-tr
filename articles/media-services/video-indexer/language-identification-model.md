---
title: Konuşulan dilleri otomatik olarak tanımlamak için Video Dizinleyici'yi kullanma - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Video Indexer dil tanımlama modelinin bir videodaki konuşulan dili otomatik olarak tanımlamak için nasıl kullanıldığı açıklanmaktadır.
services: media-services
author: juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/12/2020
ms.author: ellbe
ms.openlocfilehash: ba1521581316d559eb4e67bafba0061c31cc666b
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272959"
---
# <a name="automatically-identify-the-spoken-language-with-language-identification-model"></a>Dil tanımlama modeliyle konuşulan dili otomatik olarak tanımlama

Video Indexer otomatik dil tanımlama (LID), hangi otomatik olarak ses ten konuşulan dil içeriği tanımlama ve baskın tanımlanmış dilde transkripsiyonu için medya dosyası gönderme işlemidir destekler. 

Şu anda LID destekler: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Mandarin Chines, Japonca, Rusça ve Portekizce (Brezilyaca). 

Aşağıdaki [Yönergeler ve sınırlamalar](#guidelines-and-limitations) bölümünü gözden geçirin.

## <a name="choosing-auto-language-identification-on-indexing"></a>Dizin oluşturmada otomatik dil tanımlama seçimi

API'yi kullanarak bir videoyu dizine ekinlerken veya `sourceLanguage` [yeniden dizine](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) alırken, parametredeki `auto detect` seçeneği seçin.

Portalı kullanırken, [Video Indexer](https://www.videoindexer.ai/) ana sayfasındaki **Hesap videolarınıza** gidin ve yeniden dizine almak istediğiniz videonun adının üzerinde durun. Sağ alt köşede yeniden dizin düğmesini tıklatın. Yeniden **dizin video** iletişim kutusunda, Video **kaynak dili** açılır kutusundan *Otomatik algılama'yı* seçin.

![otomatik algılama](./media/language-identification-model/auto-detect.png)

## <a name="model-output"></a>Model çıkışı

Video Indexer, o dilin güveni `> 0.6`en olası dile göre videoyu kopyalar. Dil güvenle tanımlanamıyorsa, konuşulan dilin İngilizce olduğunu varsayar. 

Model baskın dil öznitelik olarak `sourceLanguage` (kök / video / anlayışlar altında) anlayışlar JSON mevcuttur. Öznitelik altında `sourceLanguageConfidence` karşılık gelen bir güven puanı da mevcuttur.

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

* Otomatik dil tanımlama (LID) aşağıdaki dilleri destekler: 

    İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Mandarin Chines, Japonca, Rusça ve Portekizce (Brezilyaca).
* Video Indexer Arapça (Modern Standart ve Levanten), Hintçe ve Korece'yi desteklese de, bu diller LID'de desteklenmez.
* Ses yukarıda desteklenen liste dışında diller içeriyorsa, sonuç beklenmeyen.
* Video Indexer yeterince yüksek bir güven ile`>0.6`dil tanımlayamıyorsa ( ), geri dönüş dili İngilizce'dir.
* Karışık dillerses içeren dosya için geçerli bir destek yoktur. Ses karışık diller içeriyorsa, sonuç beklenmeyen bir durumdur. 
* Düşük kaliteli ses model sonuçlarını etkileyebilir.
* Model, seste en az bir dakikalık konuşma gerektirir.
* Model spontan bir konuşma konuşma tanımak için tasarlanmıştır (değil sesli komutları, şarkı, vb).

## <a name="next-steps"></a>Sonraki adımlar

* [Genel bakış](video-indexer-overview.md)
* [Çok dilli içeriği otomatik olarak tanımlayabilir ve yazıya döktü](multi-language-identification-transcription.md)
