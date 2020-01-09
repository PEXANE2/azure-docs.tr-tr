---
title: Azure Stream Analytics 'de JSON ve AVRO ayrıştırma
description: Bu makalede, diziler, JSON, CSV biçimli veriler gibi karmaşık veri türlerinde nasıl işlem yapılacağı açıklanır.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 1741510c7398ce74da81f006cb4109d9a33f8f9f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431601"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Azure Stream Analytics JSON ve avro verilerini ayrıştırın

CSV, JSON ve avro veri biçimlerinde olayları işlemeyi destekler Azure Stream Analytics. Hem JSON hem de avro verileri yapılandırılabilir ve iç içe geçmiş nesneler (kayıtlar) ve diziler gibi bazı karmaşık türleri içerebilir. 




## <a name="record-data-types"></a>Veri türlerini Kaydet
Kayıt veri türleri, giriş veri akışlarında karşılık gelen biçimler kullanıldığında JSON ve avro dizilerini temsil etmek için kullanılır. Bu örneklerde, giriş olaylarını JSON biçiminde okuyan örnek bir algılayıcı gösterilmektedir. Tek bir olay örneği aşağıda verilmiştir:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```


### <a name="access-nested-fields-in-known-schema"></a>Bilinen şemada iç içe geçmiş alanlara erişin
Doğrudan Sorgunuzla iç içe geçmiş alanlara kolayca erişmek için nokta gösterimini (.) kullanın. Örneğin, bu sorgu önceki JSON verilerinde Location özelliği altındaki Enlem ve boylam koordinatlarını seçer. Nokta gösterimi aşağıda gösterildiği gibi birden çok düzeyde gezinmek için kullanılabilir.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.SensorMetadata.Version
FROM input
```

### <a name="select-all-properties"></a>Tüm Özellikleri Seç
' * ' Joker karakterini kullanarak iç içe geçmiş bir kaydın tüm özelliklerini seçebilirsiniz. Aşağıdaki örnek göz önünde bulundurun:

```SQL
SELECT input.Location.*
FROM input
```

Sonuç:

```json
{
    "Lat" : 47,
    "Long" : 122
}
```


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Özellik adı bir değişken olduğunda iç içe alanlara erişin
Özellik adı bir değişken ise [Getrecordpropertyvalue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue) işlevini kullanın. 

Örneğin, bir örnek veri akışının her bir cihaz algılayıcısı için eşikler içeren başvuru verileriyle katılması gerektiğini düşünün. Bu tür başvuru verilerinin bir parçacığı aşağıda gösterilmiştir.

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
    -- the where statement selects the property value coming from the reference data
```

### <a name="convert-record-fields-into-separate-events"></a>Kayıt alanlarını ayrı olaylara Dönüştür
Kayıt alanlarını ayrı olaylara dönüştürmek için, [Getrecordproperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics) Işleviyle birlikte [Apply](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işlecini kullanın. Örneğin, önceki örnekte Sensorokumak için birkaç kayıt varsa, aşağıdaki sorgu farklı olaylara ayıklamak için kullanılabilir:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```



## <a name="array-data-types"></a>Dizi veri türleri

Dizi veri türleri sıralı değerler koleksiyonudur. Dizi değerlerinde bazı tipik işlemler aşağıda ayrıntılı olarak verilmiştir. Bu örneklerde, giriş olaylarının, dizi veri türü olan "arrayField" adlı bir özelliği olduğunu varsaymaktadır.

Bu örnekler, [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [getarrayelements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics)ve [Apply](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci işlevlerini kullanır.

### <a name="working-with-a-specific-array-element"></a>Belirli bir dizi öğesiyle çalışma
Belirtilen dizinde dizi öğesi seçin (ilk dizi öğesini seçerek):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

### <a name="select-array-length"></a>Dizi uzunluğunu seçin

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

### <a name="convert-array-elements-into-separate-events"></a>Dizi öğelerini ayrı olaylara Dönüştür
Tek tek olaylar olarak tüm dizi öğelerini seçin. [Getarrayelements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) yerleşik işlevi Ile birlikte [Apply](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci, tüm dizi öğelerini ayrı olaylar olarak ayıklar:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```


## <a name="see-also"></a>Ayrıca Bkz.
[Azure Stream Analytics veri türleri](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
