---
title: Azure Medya Hizmetleri canlı olay hata kodları | Microsoft Dokümanlar
description: Bu makalede, canlı olay hata kodları listelen.
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
ms.date: 02/25/2020
ms.author: juliako
ms.openlocfilehash: f9067562f67190b8bc04392f33078d4d3262f986
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654588"
---
# <a name="media-services-live-event-error-codes"></a>Medya Hizmetleri Canlı Olay hata kodları

Bu bölümdeki tablolar [Canlı Olay](live-events-outputs-concept.md) hata kodlarını listele.

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionReddedildi

Canlı bir etkinlik için [Olay Izgara](https://docs.microsoft.com/azure/event-grid/) etkinliklerine abone olduğunuzda, [LiveEventConnectionRejected](media-services-event-schemas.md#liveeventconnectionrejected) etkinliğinden aşağıdaki hatalardan birini görebilirsiniz.

| Sonuç kodu | Açıklama |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | URL'yi yanlış yut |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Encoder IP IP izin listesinde yapılandırılan mevcut değildir |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | RTMP kodlayıcısı setDataFrame komutunu göndermedi. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Belirtilen Codec desteklenmez. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED |Medya açıklaması bilgileri, gerçek medya verileri teslim edilmeden önce alınmadı.|
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED |Ses veya video türü için niteliklerin sayısı izin verilen maksimum sınırı aştı.|
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED |Canlı bir olayveya kanal hizmetinde gelen toplam bit hızı, izin verilen maksimum sınırı aştı.|
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Video veya ses FLVTag için zaman damgası RTMP kodlayıcı geçersizdir. |
| MPE_INGEST_FRAMERATE_EXCEEDED | Gelen kodlayıcı kare hızları ile gelen kodlayıcı canlı olayları / kanalları kodlamak için izin verilen maksimum 30fps aştı.|
| MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED | Gelen kodlayıcı akışları aşağıdaki izin verilen kararları aştı: 1920x1088 canlı olaylar / kanallar kodlama kkodlama küçesi ve 4096 x 2160 geçiş canlı olaylar / kanallar için.|

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderBağlantısı kesildi

[LiveEventEncoderDisconnected](media-services-event-schemas.md#liveeventencoderdisconnected) olayında aşağıdaki hatalardan birini görebilirsiniz.

|Sonuç kodu|Açıklama|
|---|---|
|MPE_RTMP_SESSION_IDLE_TIMEOUT|RTMP oturumu izin verilen zaman sınırı için boşta kaldıktan sonra zaman doldu.|
|MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID|Video veya ses FLVTag için zaman damgası RTMP kodlayıcı geçersizdir.|
|MPE_CAPACITY_LIMIT_REACHED|Kodlayıcı çok hızlı veri gönderiyor.|
|Bilinmeyen hata kodları|Bu hata kodları, bellek hatasından karma haritadaki yinelenen girişlere kadar değişebilir.|


## <a name="see-also"></a>Ayrıca bkz.

[Akış Bitiş Noktası (Başlangıç) hata kodları](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Medya Hizmetleri ile canlı yayın](stream-live-tutorial-with-api.md)
