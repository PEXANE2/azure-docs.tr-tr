---
title: Azure Media Services Video Indexer sürüm notları | Microsoft Docs
description: En son gelişmelerden haberdar olmak için, bu makalede Azure Media Services Video Indexer en son güncelleştirmeleri sunulmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 07/22/2019
ms.author: juliako
ms.openlocfilehash: b627a78edef1c0b0fe6b3ed011678145aea397ae
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845893"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer sürüm notları

İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlev

## <a name="july-2019"></a>2019 Temmuz

### <a name="editor-as-a-widget"></a>Pencere öğesi olarak düzenleyici

Video Indexer AI Düzenleyicisi, artık müşteri uygulamalarına katıştırılacak bir pencere öğesi olarak sunulmaktadır.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Özel dil modelini portaldan kapalı açıklamalı altyazı dosyasından Güncelleştir

Müşteriler, portalın Özelleştirme sayfasında dil modelleri için giriş olarak VTT, SRT ve TTML dosya biçimleri sağlayabilir.

## <a name="june-2019"></a>Haziran 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer Japonya Doğu dağıtıldı

Artık Japonya Doğu bölgesinde Video Indexer ücretli bir hesap oluşturabilirsiniz.

### <a name="create-and-repair-account-api-preview"></a>Hesap API 'SI oluşturma ve onarma (Önizleme)

[Azure Media Service bağlantı noktasını veya anahtarını güncelleştirmenizi](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag)sağlayan yenı bir API eklendi.

### <a name="improve-error-handling-on-upload"></a>Karşıya yükleme sırasında hata işlemeyi geliştirme 

Temel alınan Azure Media Services hesabının yanlış yapılandırılması durumunda açıklayıcı bir ileti döndürülür.

### <a name="player-timeline-keyframes-preview"></a>Oyuncu zaman çizelgesi ana kare önizlemesi 

Artık Player 'ın zaman çizelgesinde her seferinde bir görüntü önizlemesi görebilirsiniz.

### <a name="editor-semi-select"></a>Düzenleyici yarı seçim

Artık düzenleyicide belirli bir öngörü zaman dilimini seçme sonucu olarak seçilen tüm öngörülerin önizlemesini görebilirsiniz.

## <a name="may-2019"></a>Mayıs 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Özel dil modelini kapalı açıklamalı altyazı dosyasından Güncelleştir

[Özel dil modeli oluşturma](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) ve [özel dil modellerini güncelleştirme](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) API 'leri artık dil modellerı için girdi olarak VTT, SRT ve ttml dosya biçimlerini desteklemektedir.

[Update video döküm API 'si](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript)çağrılırken, döküm otomatik olarak eklenir. Videoyla ilişkili eğitim modeli de otomatik olarak güncelleştirilir. Dil modellerinizi özelleştirme ve eğitme hakkında daha fazla bilgi için bkz. [video Indexer bir dil modelini özelleştirme](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Yeni karşıdan yükleme TRANSCRIPT biçimleri – TXT ve CSV

Zaten desteklenen (SRT, VTT ve TTML) kapalı açıklamalı alt yazı biçiminin yanı sıra, Video Indexer artık kodu TXT ve CSV biçimlerinde indirmeyi desteklemektedir.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
