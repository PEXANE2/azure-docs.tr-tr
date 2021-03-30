---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 91141e314bf276d9138157c8a9d85d5262ac5907
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "67188129"
---
Özel ölçümlerin toplanması. Telemetri öğesiyle ilişkilendirilmiş adlandırılmış ölçüyü raporlamak için bu koleksiyonu kullanın. Genellikle kullanım örnekleri şunlardır:
- Bağımlılık telemetri yükünün boyutu
- Istek Telemetriyle işlenen kuyruk öğelerinin sayısı
- Sihirbazın sihirbaz adımı tamamlama olayı telemetrisi adımını tamamlamak için geçen süre.

Uygulama analizinden [özel ölçümleri](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) sorgulayabilirsiniz:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Özel ölçümler, ait oldukları telemetri öğesiyle ilişkilendirilir. Bunlar, bu ölçümleri içeren telemetri öğesiyle örneklemeye tabidir. Diğer telemetri türlerinden bağımsız bir değere sahip bir ölçümü izlemek için [ölçüm telemetrisi](../articles/azure-monitor/app/api-custom-events-metrics.md)kullanın.

En büyük anahtar uzunluğu: 150
