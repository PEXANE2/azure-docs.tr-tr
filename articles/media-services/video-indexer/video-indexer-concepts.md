---
title: Video Dizinleyici kavramları
titleSuffix: Azure Media Services
description: Bu makalede, Azure Medya Hizmetleri Video Dizinleyici hizmetinin bazı kavramları açıklanmaktadır.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 73dad1db4f44134f871c9f3d6e7edcdd3bd1e2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900685"
---
# <a name="video-indexer-concepts"></a>Video Dizinleyici kavramları
 
Bu makalede, Video Dizinleyici hizmetinin bazı kavramları açıklanmaktadır.
    
## <a name="summarized-insights"></a>Özetlenmiş öngörüler

Özetlenmiş öngörüler verilerin toplu bir görünümünü içerir: yüzler, konular, duygular. Örneğin, binlerce zaman aralığının her birini üzerinden almak ve içinde hangi yüzlerin olduğunu kontrol etmek yerine, özetlenen öngörüler tüm yüzleri içerir ve her biri için, içinde göründüğü zaman aralıkları ve gösterildiği sürenin %'si.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ve ayarlanmış zaman aralığı

TimeRange orijinal videodaki zaman aralığıdır. AdjustedTimeRange geçerli çalma listesine göre zaman aralığıdır. Farklı video satırlarından bir oynatma listesi oluşturabileceğinizden, 1 saatlik bir video alabilir ve 10:00-10:15 gibi, ondan sadece 1 satır kullanabilirsiniz. Bu durumda, zaman aralığı 10:00-10:15 ama ayarlanmışTimeRange 00:00-00:15 olduğu 1 satırlı bir çalma listesi olacaktır.
 
## <a name="blocks"></a>Bloklar

Bloklar, verileri gözden geçirmeyi kolaylaştırmak içindir. Örneğin, blok ne zaman konuşmacının değiştiğine ya da uzun duraklamalar olduğuna bağlı olarak çözümlenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl başlayacağınız hakkında bilgi için, [ilk videonuza nasıl kaydolun ve yükleyebilirsiniz.](video-indexer-get-started.md)

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md)
