---
title: dosya dahil etme
description: dosya dahil etme
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 07/09/2020
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.custom: include file
ms.openlocfilehash: 7259e1981f873c8385a02fe4f353dcdda495f823
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91287431"
---
### <a name="property-limits"></a>Özellik sınırları

Azure Time Series Insights Özellik sınırları, Gen1 içinde en fazla 800 ' den 1.000 ' e artmıştır. Sağlanan olay özelliklerinin karşılık gelen JSON, CSV ve grafik sütunları [Azure Time Series Insights Gen2 Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart)içinde görüntüleyebilmeniz gerekir.

| SKU | En yüksek özellikler |
| --- | --- |
| Gen2 (L1) | 1.000 Özellikleri (sütun) |
| Gen1 (S1) | 600 özellikleri (sütun) |
| Gen1 (S2) | 800 Özellikleri (sütun) |

### <a name="streaming-ingestion"></a>Akış Alımı

* Ortam başına en fazla iki [olay kaynağı](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md) vardır.

* Olay kaynakları için en iyi uygulamalar ve genel rehberlik [burada](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices) bulunabilir

* Varsayılan olarak, Azure Time Series Insights Gen2, gelen verileri **Azure Time Series Insights Gen2 ortamı başına saniyede 1 megabayta (Mbps) kadar**bir hızda alabilir. [Hub bölümü başına](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md#hub-partitions-and-per-partition-limits)ek sınırlamalar vardır. 8 MB/sn 'ye kadar olan ücretler Azure portal aracılığıyla bir destek bileti gönderilerek sağlayabilirsiniz. Daha fazla bilgi edinmek için [akış alma performansı sınırlarını](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md)okuyun.

### <a name="api-limits"></a>API sınırları

Azure Time Series Insights Gen2 için REST API sınırları [REST API başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview#limits-1)belirtilir.
