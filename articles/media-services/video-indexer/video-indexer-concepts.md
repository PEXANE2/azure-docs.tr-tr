---
title: Video Indexer kavramlar
titleSuffix: Azure Media Services
description: Bu makalede Azure Media Services Video Indexer hizmetinin bazı kavramları açıklanmaktadır.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 73dad1db4f44134f871c9f3d6e7edcdd3bd1e2ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74900685"
---
# <a name="video-indexer-concepts"></a>Video Indexer kavramlar
 
Bu makalede Video Indexer hizmetinin bazı kavramları açıklanmaktadır.
    
## <a name="summarized-insights"></a>Özetlenen Öngörüler

Özetlenen Öngörüler, verilerin toplu bir görünümünü içerir: yüzler, konular, lamalar. Örneğin, binlerce zaman aralığının her birini ve hangi yüzlerin nerede olduğunu kontrol etmek yerine, özetlenen Öngörüler tüm yüzleri ve her biri için, göründüğü zaman aralıklarını ve gösterilen sürenin yüzdesini içerir.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ve ayarlanan zaman aralığı karşılaştırması

TimeRange, orijinal videodaki zaman aralığıdır. Ayarlantedtimerange, geçerli çalma listesine göre zaman aralığıdır. Farklı videoların farklı satırlarından bir çalma listesi oluşturbileceğinizden, 1 saatlik bir video alabilir ve yalnızca 1 satır kullanabilirsiniz, örneğin, 10:00-10:15. Bu durumda, 1 satır içeren bir çalma listenize sahip olursunuz, burada zaman aralığı 10:00-10:15, ancak Ayarlantedtimerange 00:00-00:15 ' dir.
 
## <a name="blocks"></a>Bloklar

Bloklar, verileri daha kolay hale getirmek için tasarlanmıştır. Örneğin, blok ne zaman konuşmacının değiştiğine ya da uzun duraklamalar olduğuna bağlı olarak çözümlenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama hakkında daha fazla bilgi için bkz. [nasıl kaydolun ve ilk videonuzu karşıya yükleyin](video-indexer-get-started.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md)
