---
title: Desteklenen veri türleri-Azure Time Series Insights Gen2 | Microsoft Docs
description: Azure Time Series Insights Gen2 'de desteklenen veri türleri hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 9d29d83ed92ee0fa2217bb1c27fbf6c2fbb3584c
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87170872"
---
# <a name="supported-data-types"></a>Desteklenen veri türleri

Aşağıdaki tabloda Azure Time Series Insights Gen2 tarafından desteklenen veri türleri listelenmektedir

| Veri türü | Açıklama | Örnek | Parquet içindeki özellik sütunu adı
|---|---|---|---|
| **bool** | İki durumdan birine sahip bir veri türü: `true` veya `false` . | `"isQuestionable" : true` | isQuestionable_bool
| **hem** | Genellikle günün tarih ve saati olarak ifade edilen bir anlık zamanı temsil eder. [Iso 8601](https://www.iso.org/iso-8601-date-and-time-format.html) biçiminde ifade edilir. DateTime özellikleri her zaman UTC biçiminde depolanır. Doğru biçimlendirildiyse, saat dilimi uzaklıkları uygulanır ve ardından UTC 'de depolanır. Ortam zaman damgası özelliği ve tarih saat uzaklıkları hakkında daha fazla bilgi için [Bu](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) bölüme bakın | `"eventProcessedLocalTime": "2020-03-20T09:03:32.8301668Z"` | eventProcessedLocalTime_datetime
| **double** | Çift duyarlıklı 64 bit sayı  | `"value": 31.0482941` | value_double
| **long** | İmzalı 64 bitlik bir tamsayı  | `"value" : 31` | value_long
| **dizisinde** | Metin değerleri geçerli UTF-8 içermelidir. Null ve boş dizeler aynı şekilde işlenir. |  `"site": "DIM_MLGGG"` | site_string
| **dinamik** | Dizi veya özellik çantasından (sözlük) oluşan karmaşık (basit olmayan) bir tür. Şu anda yalnızca, TS ID veya TimeStamp Özelliği (ies) içermeyen nesnelerin basit veya dizi dizilerinin strıngımı JSON dizileri dinamik olarak depolanacak. Nesnelerin düzleştirilmesini ve dizilerin nasıl toplanacağını anlamak için bu [makaleyi](./concepts-json-flattening-escaping-rules.md) okuyun. Bu tür olarak depolanan yük özelliklerine Azure Time Series Insights Gen2 Explorer ve `GetEvents`   sorgu API 'si aracılığıyla erişilebilir. |  `"values": "[197, 194, 189, 188]"` | values_dynamic

## <a name="sending-mixed-data-types"></a>Karışık veri türleri gönderme

Azure Time Series Insights Gen2 ortamınız kesin bir şekilde türdedir. Cihazlar veya Etiketler bir cihaz özelliği için farklı türlerdeki verileri gönderse, değerler iki ayrı sütunda depolanır ve API çağrılarında zaman serisi model değişkeni ifadelerinizi tanımlarken [birleşim () işlevi](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions) kullanılmalıdır.

Azure Time Series Insights Gezgini, aynı cihaz özelliğinin ayrı sütunlarını otomatik olarak eklemenin bir yolunu sunar. Aşağıdaki örnekte, algılayıcı `PresentValue` hem Long hem de Double olabilecek bir özellik gönderir. Özelliğin tüm depolanan değerlerle (veri türünden bağımsız olarak) `PresentValue` sorgulanmak için, öğesini seçin `PresentValue (Double | Long)` ve sütunlar sizin için birleştirme yapılır.

[![Gezgin otomatik birleşim](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png)](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png#lightbox)

## <a name="objects-and-arrays"></a>Nesneler ve diziler

Olay yüklerinizin bir parçası olarak nesneler ve diziler gibi karmaşık türler gönderebilirsiniz. İç içe geçmiş nesneler düzleştirilir ve diziler, `dynamic` ortam yapılandırmanıza ve JSON şekline bağlı olarak birden çok olay oluşturmak üzere olarak depolanır veya düzleştirilir. [JSON düzleştirme ve kaçış kuralları](./concepts-json-flattening-escaping-rules.md) hakkında daha fazla bilgi edinmek için

## <a name="next-steps"></a>Sonraki adımlar

* Olayların nasıl depolanacağını anlamak için [JSON düzleştirme ve kaçış kurallarını](./concepts-json-flattening-escaping-rules.md) okuyun.

* Ortamınızın [verimlilik sınırlamalarını](./concepts-streaming-ingress-throughput-limits.md) anlayın

* Akış verilerini almak için [olay kaynakları](concepts-streaming-ingestion-event-sources.md) hakkında bilgi edinin.
