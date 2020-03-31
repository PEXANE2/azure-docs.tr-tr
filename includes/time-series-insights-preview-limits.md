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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77123234"
---
### <a name="general-availability-and-preview-comparison"></a>Genel kullanılabilirlik ve önizleme karşılaştırması

Aşağıdaki tablo, Azure Zaman Serisi Öngörüleri genel kullanılabilirliği (GA) ve önizleme örnekleri arasındaki birkaç önemli farkı özetleyin.

| | GA | Önizleme |
| --- | --- | ---|
| Birinci sınıf vatandaş | Olay merkezli | Zaman serisi merkezli |
| Anlamsal akıl yürütme | Düşük düzey (başvuru verileri) | Üst düzey (modeller) |
| Veri bağlamsallaştırma | Cihaz dışı düzey | Cihaz ve cihaz dışı seviye |
| Bilgi işlem mantığı depolama | Hayır | Modelin tip değişkenleri kısmında depolanır |
| Depolama ve erişim kontrolü | Hayır | Model üzerinden etkin |
| Toplamalar/Örnekleme | Hayır | Olay ağırlıklı ve zaman ağırlıklı |
| Sinyal rekonstrüksiyonu | Hayır | Enterpolasyon |
| Türetilmiş zaman serilerinin üretimi | Hayır | Evet, birleştirir ve birleştirir |
| Dil esnekliği | Tek kullanımlık olmayan | Tek kullanımlık |
| İfade dili | Yüklem dizesi | Zaman serisi ifadeleri (yüklem dizeleri, değerler, ifadeler ve işlevler) |

### <a name="property-limits"></a>Özellik sınırları

Time Series Insights özellik sınırları GA'daki maksimum 800 kapaktan 1.000'e yükselmiştir. Sağlanan olay özellikleri, [Zaman Serisi Öngörüleri Önizleme gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart)içinde görüntülenebilen ilgili JSON, CSV ve grafik sütunlarına sahiptir.

| SKU | Maksimum özellikler |
| --- | --- |
| Önizleme PAYG | 1.000 özellik (sütun) |
| GA S1 | 600 özellik (sütun) |
| GA S2 | 800 özellik (sütun) |

### <a name="event-sources"></a>Olay kaynakları

Örnek başına en fazla iki olay kaynağı desteklenir. 

* [Olay hub kaynağı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)nı nasıl ekleyeceğinizi öğrenin.
* Bir [IoT hub kaynağını](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub)yapılandırın.

Varsayılan olarak, Önizleme ortamları **çevre başına saniyede 1 megabayta (MB/s)** kadar [giriş oranlarını destekler.](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#ingress-scale-and-preview-limitations) Müşteriler, gerektiğinde Önizleme ortamlarını **16 MB/sn'ye** kadar ölçeklendirebilir. Ayrıca **0,5 MB/s'lik**bir bölüm başına limit ilerler. 

### <a name="api-limits"></a>API sınırları

Zaman Serisi Öngörüleri Önizlemesi için REST API sınırları [REST API başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits)belirtilmiştir.
