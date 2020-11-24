---
title: include dosyası
description: include dosyası
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.custom: include file
ms.openlocfilehash: e7b5dfe9380612d56e591c3f619dfcb8582f8dee
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95560030"
---
### <a name="property-limits"></a>Özellik sınırları

Azure Time Series Insights Özellik sınırları, ısınma depolama için 1.000 ' e yükselmiştir ve soğuk depolama için özellik sınırı yoktur. Sağlanan olay özelliklerinin karşılık gelen JSON, CSV ve grafik sütunları [Azure Time Series Insights Gen2 Explorer](../articles/time-series-insights/quickstart-explore-tsi.md)içinde görüntüleyebilmeniz gerekir.

| SKU | En yüksek özellikler |
| --- | --- |
| Gen2 (L1) | yarı depolama için 1.000 Özellikleri (sütunlar) ve soğuk depolama için sınırsız|
| Gen1 (S1) | 600 özellikleri (sütun) |
| Gen1 (S2) | 800 Özellikleri (sütun) |

### <a name="streaming-ingestion"></a>Akış Alımı

* Ortam başına en fazla iki [olay kaynağı](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md) vardır.

* Olay kaynakları için en iyi uygulamalar ve genel rehberlik [burada](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices) bulunabilir

* Varsayılan olarak, Azure Time Series Insights Gen2, gelen verileri **Azure Time Series Insights Gen2 ortamı başına saniyede 1 megabayta (Mbps) kadar** bir hızda alabilir. [Hub bölümü başına](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md#hub-partitions-and-per-partition-limits)ek sınırlamalar vardır. 8 MB/sn 'ye kadar olan ücretler Azure portal aracılığıyla bir destek bileti gönderilerek sağlayabilirsiniz. Daha fazla bilgi edinmek için [akış alma performansı sınırlarını](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md)okuyun.

### <a name="api-limits"></a>API sınırları

Azure Time Series Insights Gen2 için REST API sınırları [REST API başvuru belgelerinde](/rest/api/time-series-insights/preview#limits-1)belirtilir.