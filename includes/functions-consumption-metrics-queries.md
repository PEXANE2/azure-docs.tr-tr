---
ms.openlocfilehash: 0bae8946b7d81fbf45698cefe7c8b00bbc94d00c
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168128"
---
#### <a name="determine-memory-usage"></a>Bellek kullanımını belirleme 

**İzleme**altında **Günlükler (Analiz)** öğesini seçin, ardından aşağıdaki telemetri sorgusunu kopyalayın ve sorgu penceresine yapıştırın ve **Çalıştır**' ı seçin. Bu sorgu, her örneklenmiş zamanda toplam bellek kullanımını döndürür.

```
performanceCounters
| where name == "Private Bytes"
| project timestamp, name, value
```

Sonuçlar aşağıdaki örneğe benzer şekilde görünür:

| zaman damgası \[ UTC\]          | name          | değer       |
|----------------------------|---------------|-------------|
| 9/12/2019, 1:05:14 \. 947 | Özel baytlar | 209.932.288 |
| 9/12/2019, 1:06:14 \. 994 | Özel baytlar | 212.189.184 |
| 9/12/2019, 1:06:30 \. 010 | Özel baytlar | 231.714.816 |
| 9/12/2019, 1:07:15 \. 040 | Özel baytlar | 210.591.744 |
| 9/12/2019, 1:12:16 \. 285 | Özel baytlar | 216.285.184 |
| 9/12/2019, 1:12:31 \. 376 | Özel baytlar | 235.806.720 |

#### <a name="determine-duration"></a>Süreyi belirleme 

Azure Izleyici, kaynak düzeyindeki ölçümleri izler, bu Işlevler işlev uygulamadır. Application Insights tümleştirme ölçümleri işlev başına temelinde yayar. Bir işlevin ortalama süresini almak için örnek bir analiz sorgusu aşağıda verilmiştir:

```
customMetrics
| where name contains "Duration"
| extend averageDuration = valueSum / valueCount
| summarize averageDurationMilliseconds=avg(averageDuration) by name
```

| name                       | averageDurationMilliseconds |
|----------------------------|-----------------------------|
| QueueTrigger AvgDurationMs | 16 \. 087                     |
| QueueTrigger MaxDurationMs | 90 \. 249                     |
| QueueTrigger MinDurationMs | 8 \. 522                      |
