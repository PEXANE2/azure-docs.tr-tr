---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 02/27/2020
ms.author: orspodek
ms.openlocfilehash: a2297301a0b9c0540c73c0f50483cccfc3181a0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79128653"
---
### <a name="event-system-properties-mapping"></a>Olay sistemi özellikleri eşleme

> [!Note]
> * Sistem özellikleri tek kayıtlı olaylar için desteklenir.
> * Eşleme için, `csv` özellikler kaydın başına eklenir. Eşleme için, `json` özellikler açılır listede görünen ada göre eklenir.

Tablonun **Veri Kaynağı** bölümünde Olay **sistemi özelliklerini** seçtiyseniz, tablo şemasına ve eşleme eşlemelerine aşağıdaki özellikleri eklemeniz gerekir.

**Tablo şeması örneği**

Verileriniz üç sütun`Timespan`içeriyorsa (, `Metric`ve `Value`) `x-opt-enqueued-time` `x-opt-offset`ve eklediğiniz özellikler bu komutu kullanarak tablo şemasını oluşturur veya değiştirir:

```kusto
    .create-merge table TestTable (TimeStamp: datetime, Metric: string, Value: int, EventHubEnqueuedTime:datetime, EventHubOffset:string)
```

**CSV haritalama örneği**

Kaydın başına veri eklemek için aşağıdaki komutları çalıştırın. Not ordinal değerleri.

```kusto
    .create table TestTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "Timespan", "Properties":{"Ordinal":"2"}},'
    '   { "column" : "Metric", "Properties":{"Ordinal":"3"}},'
    '   { "column" : "Value", "Properties":{"Ordinal":"4"}},'
    '   { "column" : "EventHubEnqueuedTime", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "EventHubOffset", "Properties":{"Ordinal":"1"}}'
    ']'
```
 
**JSON haritalama örneği**

**Veriler, Veri bağlantı** bıçağı **Olay sistemi özellikleri** listesinde göründükleri gibi sistem özellikleri adları kullanılarak eklenir. Şu komutları çalıştırın:

```kusto
    .create table TestTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "Timespan", "Properties":{"Path":"$.timestamp"}},'
    '    { "column" : "Metric", "Properties":{"Path":"$.metric"}},'
    '    { "column" : "Value", "Properties":{"Path":"$.metric_value"}},'
    '    { "column" : "EventHubEnqueuedTime", "Properties":{"Path":"$.x-opt-enqueued-time"}},'
    '    { "column" : "EventHubOffset", "Properties":{"Path":"$.x-opt-offset"}}'
    ']'
```
