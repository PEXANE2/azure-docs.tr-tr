---
title: include dosyası
description: include dosyası
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 07/09/2020
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.custom: include file
ms.openlocfilehash: 48080bb4d1e24f7f98d3dfe1fd63b65ba46df35e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289921"
---
### <a name="property-limits"></a>Özellik sınırları

Azure Time Series Insights Özellik sınırları, Gen1 içinde en fazla 800 ' den 1.000 ' e artmıştır. Sağlanan olay özelliklerinin karşılık gelen JSON, CSV ve grafik sütunları [Azure Time Series Insights Gen2 Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart)içinde görüntüleyebilmeniz gerekir.

| SKU | En yüksek özellikler |
| --- | --- |
| Gen2 (L1) | 1.000 Özellikleri (sütun) |
| Gen1 (S1) | 600 özellikleri (sütun) |
| Gen1 (S2) | 800 Özellikleri (sütun) |

### <a name="event-sources"></a>Olay kaynakları

Örnek başına en fazla iki olay kaynağı desteklenir.

* [Bir olay hub 'ı kaynağı eklemeyi](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)öğrenin.
* [IoT Hub 'ı kaynağı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub)yapılandırın.

Varsayılan olarak, [Gen2 ortamları](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingress-throughput-limits) , **ortam başına en fazla 1 megabayt (MB/sn) saniye başına**giriş hızını destekler. Müşteriler, gerekirse ortamlarını **16 MB/sn** 'ye kadar ölçeklendirebilir. Bölüm başına **0,5 MB/sn**sınırı da vardır.

### <a name="api-limits"></a>API sınırları

Azure Time Series Insights Gen2 için REST API sınırları [REST API başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview#limits-1)belirtilir.
