---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 91141e314bf276d9138157c8a9d85d5262ac5907
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188129"
---
Özel ölçümlerin toplanması. Telemetri öğesi ile ilişkili adlandırılmış ölçümü bildirmek için bu koleksiyonu kullanın. Tipik kullanım durumları şunlardır:
- Bağımlılık Telemetri yükü boyutu
- İstek Telemetrisi tarafından işlenen sıra öğelerinin sayısı
- müşteri sihirbaz adım tamamlama Olay Telemetri adımı tamamlamak için aldı zaman.

Uygulama Analizi'nde [özel ölçümleri](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) sorgulayabilirsiniz:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Özel ölçümler, ait oldukları telemetri öğesi ile ilişkilidir. Bu ölçümleri içeren telemetri öğesi ile örnekleme tabidirler. Diğer telemetri türlerinden bağımsız bir değere sahip bir ölçümü izlemek için [Metrik telemetri'yi](../articles/azure-monitor/app/api-custom-events-metrics.md)kullanın.

Maksimum anahtar uzunluğu: 150
