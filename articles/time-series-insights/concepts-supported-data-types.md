---
title: Desteklenen veri türleri-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizlemede desteklenen veri türleri hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 1a1ab9fa19956341e930c85d13d7067be4449cea
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050101"
---
# <a name="supported-data-types"></a>Desteklenen veri türleri

Aşağıdaki tabloda Time Series Insights tarafından desteklenen veri türleri listelenmektedir

| Veri türü | Açıklama | Örnek | Parquet içindeki özellik sütunu adı
|---|---|---|---|
| **bool** | İki durumdan birine sahip bir veri türü: `true` veya `false` . | "ıssorgulanabilir": true | isQuestionable_bool
| **datetime** | Genellikle günün tarih ve saati olarak ifade edilen bir anlık zamanı temsil eder. [Iso 8601](https://www.iso.org/iso-8601-date-and-time-format.html) biçiminde ifade edilir. DateTime özellikleri her zaman UTC biçiminde depolanır. Doğru biçimlendirildiyse, saat dilimi uzaklıkları uygulanır ve ardından UTC 'de depolanır. Ortam zaman damgası özelliği ve tarih saat uzaklıkları hakkında daha fazla bilgi için [Bu](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) bölüme bakın | "eventProcessedLocalTime": "2020-03-20T09:03:32.8301668 Z" | eventProcessedLocalTime_datetime 
| **double** | Çift duyarlıklı 64 bit sayı  | "değer": 31,0482941 | value_double
| **long** | İmzalı 64 bitlik bir tamsayı  | "değer": 31 | value_long
| **string** | Metin değerleri geçerli UTF-8 içermelidir. |  "site": "DIM_MLGGG" | site_string
| **dynamic** | Dizi veya özellik çantasından (sözlük) oluşan karmaşık (basit olmayan) bir tür. Şu anda yalnızca strıngıorımi veya zaman damgası özellikleri içermeyen nesne dizileri ya da dinamik olarak depolanacak. Nesnelerin düzleştirilmesini ve dizilerin nasıl toplanacağını anlamak için bu [makaleyi](./concepts-json-flattening-escaping-rules.md) okuyun |  "Values": "[197, 194, 189, 188]" | values_dynamic

> [!IMPORTANT]
>
> * TSI ortamınız kesin olarak belirlenmiş. Cihazlar veya Etiketler hem integral hem de tamsayı olmayan veriler gönderse, cihaz özelliği değerleri iki ayrı çift ve uzun sütunlarda depolanır ve API çağrıları yapılırken ve zaman serisi model değişkeni ifadelerinizi tanımlarken [birleşim () işlevi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) kullanılmalıdır.

#### <a name="objects-and-arrays"></a>Nesneler ve diziler

Olay yüklerinizin bir parçası olarak nesneler ve diziler gibi karmaşık türler gönderebilirsiniz. İç içe geçmiş nesneler düzleştirilir ve diziler, `dynamic` ortam yapılandırmanıza ve JSON şekline bağlı olarak birden çok olay oluşturmak üzere olarak depolanır veya düzleştirilir. [JSON düzleştirme ve kaçış kuralları](./concepts-json-flattening-escaping-rules.md) hakkında daha fazla bilgi edinmek için

## <a name="next-steps"></a>Sonraki adımlar

* Olayların nasıl depolanacağını anlamak için [JSON düzleştirme ve kaçış kurallarını](./concepts-json-flattening-escaping-rules.md) okuyun. 

* Ortamınızın [verimlilik sınırlamalarını](concepts-streaming-throughput-limitations.md) anlayın

* Akış verilerini almak için [olay kaynakları](concepts-streaming-ingestion-event-sources.md) hakkında bilgi edinin.
