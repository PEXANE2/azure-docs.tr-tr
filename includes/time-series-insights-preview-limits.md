---
title: include dosyası
description: include dosyası
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/07/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 7bc6938523a6d66a2bc20b37c659568fc5ca494d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77123234"
---
### <a name="general-availability-and-preview-comparison"></a>Genel kullanılabilirlik ve önizleme karşılaştırması

Aşağıdaki tabloda Azure Time Series Insights genel kullanım (GA) ve önizleme örnekleri arasındaki birçok önemli fark özetlenmektedir.

| | GA | Önizleme |
| --- | --- | ---|
| Birinci sınıf vatandaşlık | Olay merkezli | Zaman serisi merkezli |
| Anlam mantık yürütme | Alt düzey (başvuru verileri) | Üst düzey (modeller) |
| Veri bağlamı | Cihaz dışı düzeyi | Cihaz ve cihaz dışı düzeyi |
| İşlem mantığı depolaması | Hayır | Modelin tür değişkenlerinde depolanan bölümü |
| Depolama ve erişim denetimi | Hayır | Model aracılığıyla etkinleştirildi |
| Toplamalar/örnekleme | Hayır | Olay ağırlıklı ve zaman ağırlıklı |
| Sinyal yeniden oluşturma | Hayır | İlişkilendirme |
| Türetilmiş zaman serisinin üretimi | Hayır | Evet, birleştirmeler ve birleşimler |
| Dil esnekliği | Birleştirilemeyen | Birleştirilebilir |
| İfade dili | Koşul dizesi | Zaman serisi ifadeleri (koşul dizeleri, değerler, ifadeler ve işlevler) |

### <a name="property-limits"></a>Özellik sınırları

Time Series Insights Özellik sınırları, GA 'de en fazla 800 ' den 1.000 ' e artmıştır. Sağlanan olay özelliklerinin karşılık gelen JSON, CSV ve grafik sütunları [Time Series Insights önizleme Gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart)içinde görüntüleyebilmeniz gerekir.

| SKU | En yüksek özellikler |
| --- | --- |
| Önizleme PAYG | 1.000 Özellikleri (sütun) |
| GA S1 | 600 özellikleri (sütun) |
| GA S2 | 800 Özellikleri (sütun) |

### <a name="event-sources"></a>Olay kaynakları

Örnek başına en fazla iki olay kaynağı desteklenir. 

* [Bir olay hub 'ı kaynağı eklemeyi](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)öğrenin.
* [IoT Hub 'ı kaynağı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub)yapılandırın.

Varsayılan olarak, [Önizleme ortamları](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#ingress-scale-and-preview-limitations) **ortam başına en fazla 1 megabayt (MB/sn)** hızda giriş hızını destekler. Müşteriler, gerekirse, önizleme ortamlarını **16 MB/sn** aktarım hızına kadar ölçeklendirebilir. Bölüm başına **0,5 MB/sn**sınırı da vardır. 

### <a name="api-limits"></a>API sınırları

Time Series Insights önizlemesi için REST API sınırları [REST API başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits)belirtilmiştir.
