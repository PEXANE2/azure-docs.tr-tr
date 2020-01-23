---
title: Video Indexer ve Azure Media Services v3 önayarlarının karşılaştırması
description: Bu makalede Video Indexer özellikleri ve Azure Media Services v3 önayarları karşılaştırılır.
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
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 2e2abd4ffe5a6d0a6336d811599db687da146f1e
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513193"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Azure Media Services v3 önayarlarını karşılaştırın ve Video Indexer 

Bu makalede **video Indexer API** 'lerin ve **Media Services v3 API**'lerinin özellikleri karşılaştırılır. 

Şu anda, [video Indexer API](https://api-portal.videoindexer.ai/) 'leri ve [Media Services v3 API 'leri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json)tarafından sunulan özellikler arasında bir çakışma var. Aşağıdaki tabloda, farklar ve benzerlikler anlamak için geçerli kılavuz sunulmaktadır. 

## <a name="compare"></a>Karşılaştır

|Özellik|Video Indexer API 'Leri |Video Çözümleyicisi ve ses Çözümleyicisi ön ayarları<br/>Media Services v3 API 'Lerinde|
|---|---|---|
|Media Insights|[Leştirilmiş](video-indexer-output-json-v2.md) |[Temel Konular](../latest/intelligence-concept.md)|
|Lerinde|Desteklenen özelliklerin tam listesini görüntüleyin: <br/> [Genel bakış](video-indexer-overview.md)|Yalnızca video öngörülerini döndürür|
|Faturalandırma|[Media Services fiyatlandırması](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services fiyatlandırması](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Uyumluluk|[Iso 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SOC 1, 2, 3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [Fedtrist](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci)ve [HiTRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust) sertifikalı. En güncel güncelleştirmeler için [video Indexer geçerli sertifika durumu](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)' nu ziyaret edin.|Media Services birçok sertifika ile uyumludur. [Azure uyumluluk teklifleri. PDF](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) ' ye göz atın ve ilgilendiğiniz sertifikayla uyumlu olup olmadığını görmek için "Media Services" araması yapın.|
|Ücretsiz Deneme|Doğu ABD|Kullanılamıyor|
|Bölgelere göre kullanılabilirlik|Doğu ABD 2, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa, Güneydoğu Asya, Doğu Asya ve Avustralya Doğu.  En güncel güncelleştirmeler için, [bölgelere göre ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) sayfasını ziyaret edin.|Bkz. [Azure durumu](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)

[Media Services v3 genel bakış](../latest/media-services-overview.md)
