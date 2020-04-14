---
title: include dosyası
description: include dosyası
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 11469d992e0f5669cd3fc1e3864627dd0b8ae23d
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263361"
---
Genel Kullanılabilirlik'teki temel sınırları aşağıda özetler.

### <a name="sku-ingress-rates-and-capacities"></a>SKU giriş oranları ve kapasiteleri

S1 ve S2 SKU giriş hızları ve kapasiteleri, yeni bir Time Series Insights ortamını yapılandırırken esneklik sağlar. SKU kapasiteniz, hangisi önce gelirse, depolanan olay veya bayt sayısına bağlı olarak günlük giriş oranınızı gösterir. Girişin dakika *başına*ölçüldüğünde ve belirteç kovaalgoritması kullanılarak **daraltma** uygulandığını unutmayın. Giriş 1-KB bloklarda ölçülür. Örneğin, 0,8-KB gerçek olay tek bir olay olarak ölçülür ve 2.6-KB olayı üç olay olarak sayılır.

| S1 SKU kapasitesi | Giriş hızı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | Günde 1 GB (1 milyon etkinlik) | Ayda 30 GB (30 milyon etkinlik) |
| 10 | Günde 10 GB (10 milyon etkinlik) | Ayda 300 GB (300 milyon etkinlik) |

| S2 SKU kapasitesi | Giriş hızı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | Günde 10 GB (10 milyon etkinlik) | Ayda 300 GB (300 milyon etkinlik) |
| 10 | Günde 100 GB (100 milyon etkinlik) | Ayda 3 TB (3 milyar etkinlik) |

> [!NOTE]
> Kapasiteler doğrusal ölçeklendirilir, bu nedenle 2 kapasiteli bir S1 SKU günlük giriş oranını 2 GB (2 milyon) ve ayda 60 GB (60 milyon etkinlik) etkinliği destekler.

S2 SKU ortamları ayda önemli ölçüde daha fazla etkinliği destekler ve giriş kapasitesini önemli ölçüde daha yüksek tir.

| SKU  | Aylık olay sayısı  | Dakika başına olay sayısı | Dakika başına olay boyutu  |
|---------|---------|---------|---------|---------|
| S1     |   30 milyon   |  720    |  720 KB   |
 |S2     |   300 milyon   | 7,200   | 7.200 KB  |

### <a name="property-limits"></a>Özellik sınırları

GA özellik sınırları seçilen SKU ortamına bağlıdır. Sağlanan olay özellikleri, [Zaman Serisi Öngörüler Gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart)içinde görüntülenebilen ilgili JSON, CSV ve grafik sütunlarına sahiptir.

| SKU | Maksimum özellikler |
| --- | --- |
| S1 | 600 özellik (sütun) |
| S2 | 800 özellik (sütun) |

### <a name="event-sources"></a>Olay kaynakları

Örnek başına en fazla iki olay kaynağı desteklenir. 

* [Olay hub kaynağı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)nı nasıl ekleyeceğinizi öğrenin.
* Bir [IoT hub kaynağını](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub)yapılandırın.

### <a name="api-limits"></a>API sınırları

Time Series Insights Genel Kullanılabilirliği için REST API sınırları [REST API referans belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits)belirtilmiştir.