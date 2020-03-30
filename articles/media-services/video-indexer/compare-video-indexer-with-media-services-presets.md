---
title: Video Dizinleyici ve Azure Medya Hizmetleri v3 hazır ayarlarının karşılaştırılması
description: Bu makalede, Video Dizinleyici yetenekleri ve Azure Media Services v3 hazır ayarları karşılaştırılır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: dcfc6ea4afe23424e72c625518356be52f62bc81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77602196"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Azure Medya Hizmetleri v3 hazır ayarlarını ve Video Dizinleyici'yi karşılaştırın 

Bu makalede, **Video Dizinleyici API'ler** ve **Medya Hizmetleri v3 API'lerinin**yetenekleri karşılaştırılır. 

Şu anda, [Video Dizinleyici API'ler](https://api-portal.videoindexer.ai/) ve [Medya Hizmetleri v3 API'leri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json)tarafından sunulan özellikler arasında bir çakışma vardır. Aşağıdaki tablo, farklılıkları ve benzerlikleri anlamak için geçerli kılavuzu sunar. 

## <a name="compare"></a>Karşılaştır

|Özellik|Video Dizinleyici API'ler |Video Analizörü ve Ses Analizörü Hazır Ayarları<br/>medya hizmetleri v3 API'larda|
|---|---|---|
|Medya Öngörüleri|[Gelişmiş](video-indexer-output-json-v2.md) |[Temel Konular](../latest/intelligence-concept.md)|
|Deneyim|Desteklenen özelliklerin tam listesine bakın: <br/> [Genel bakış](video-indexer-overview.md)|Yalnızca video istatistiklerini döndürür|
|Faturalandırma|[Medya Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Medya Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Uyumluluk|En güncel uyumluluk güncelleştirmeleri için [Azure Uyumluluk Teklifleri.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) adresini ziyaret edin ve ilgi alanı sertifikasına uyup uymadığını görmek için "Video Indexer"ı arayın.|En güncel uyumluluk güncelleştirmeleri için [Azure Uyumluluk Teklifleri.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) adresini ziyaret edin ve ilgi alanı sertifikasına uyup uymadığını görmek için "Medya Hizmetleri"ni arayın.|
|Ücretsiz Deneme|Doğu ABD|Kullanılamaz|
|Bölge kullanılabilirliği|Bölgelere [göre Bilişsel Hizmetler kullanılabilirliğine bakın](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)|[Bölgelere göre Medya Hizmetleri kullanılabilirliğine](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)bakın.|

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)

[Medya Hizmetleri v3 genel bakış](../latest/media-services-overview.md)
