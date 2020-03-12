---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 02/27/2020
ms.author: orspodek
ms.openlocfilehash: a2297301a0b9c0540c73c0f50483cccfc3181a0f
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128653"
---
### <a name="event-system-properties-mapping"></a>Olay sistemi özellikleri eşleme

> [!Note]
> * Sistem Özellikleri, tek kayıtlı olaylar için desteklenir.
> * `csv` eşleme için, Özellikler kaydın başına eklenir. `json` eşleme için, Özellikler açılan listede görünen ada göre eklenir.

Tablonun **veri kaynağı** bölümünde **olay sistem özellikleri** ' ni seçtiyseniz, tablo şemasında ve eşlemede aşağıdaki özellikleri eklemeniz gerekir.

**Tablo şeması örneği**

Verileriniz üç sütun (`Timespan`, `Metric`ve `Value`) içeriyorsa ve dahil ettiğiniz Özellikler `x-opt-enqueued-time` ve `x-opt-offset`ise, bu komutu kullanarak tablo şemasını oluşturun veya değiştirin:

```kusto
    .create-merge table TestTable (TimeStamp: datetime, Metric: string, Value: int, EventHubEnqueuedTime:datetime, EventHubOffset:string)
```

**CSV eşleme örneği**

Kayıt başlangıcına veri eklemek için aşağıdaki komutları çalıştırın. Notsırası değerleri.

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
 
**JSON eşleme örneği**

Veriler, **veri bağlantısı** dikey penceresi **olay sistemi özellikleri** listesinde göründükleri gibi sistem özellikleri adları kullanılarak eklenir. Şu komutları çalıştırın:

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
