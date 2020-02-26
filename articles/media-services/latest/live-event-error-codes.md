---
title: Canlı olay hata kodları Azure Media Services | Microsoft Docs
description: Bu makalede, canlı olay hata kodları listelenmektedir.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2020
ms.author: juliako
ms.openlocfilehash: bfbef771d33ad4d63ec8eaef83331e497d476071
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599476"
---
# <a name="media-services-live-event-error-codes"></a>Media Services canlı olay hata kodları

Aşağıdaki tabloda, [canlı olay](live-events-outputs-concept.md) hata kodları listelenmektedir:

|Hata|Açıklama|
|---|---| 
|MPE_INGEST_FRAMERATE_EXCEEDED|Bu hata, gelen kodlayıcı canlı olayları/kanalları kodlamak için 30 fps 'yi aşan akışlar gönderirken oluşur.|
|MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED|Bu hata, gelen kodlayıcı şu çözümleri aşan akışlar gönderirken oluşur: canlı olayları/kanalları kodlamak için 1920x1088 ve doğrudan geçiş canlı olayları/kanalları için 4096 x 2160.|

## <a name="see-also"></a>Ayrıca bkz.

[Akış uç noktası (başlangıç) hata kodları](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Media Services ile canlı akış](stream-live-tutorial-with-api.md)
