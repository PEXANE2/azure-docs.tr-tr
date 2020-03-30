---
title: Azure Stream Analytics'te JSON ve AVRO'nun ayrıştı
description: Bu makalede, diziler, JSON, CSV biçimlendirilmiş veriler gibi karmaşık veri türlerinde nasıl çalışılabilen ler açıklanmaktadır.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 01/29/2020
ms.openlocfilehash: 73905483850a47a9d036bef1b9e1ee60d3484555
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484596"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Azure Akış Analizi'nde Ayrışma JSON ve Avro verileri

Azure Akış Analizi, CSV, JSON ve Avro veri biçimlerindeki işleme olaylarını destekler. Hem JSON hem de Avro verileri yapılandırılmış olabilir ve iç içe nesneler (kayıtlar) ve diziler gibi bazı karmaşık türleri içerebilir. 

>[!NOTE]
>Event Hub Capture tarafından oluşturulan AVRO dosyaları, özel *deserializer* özelliğini kullanmanızı gerektiren belirli bir biçim kullanır. Daha fazla bilgi için bkz. [.NET özel deserializers kullanarak herhangi bir biçimde giriş oku.](https://docs.microsoft.com/azure/stream-analytics/custom-deserializer-examples)



## <a name="record-data-types"></a>Veri türlerini kaydetme
Giriş veri akışlarında karşılık gelen biçimler kullanıldığında, kayıt veri türleri JSON ve Avro dizilerini temsil etmek için kullanılır. Bu örnekler, giriş olaylarını JSON formatında okuyan bir örnek sensörü gösterir. Aşağıda tek bir olay örneği verilmiştir:

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

### <a name="access-nested-fields-in-known-schema"></a>Bilinen şemada iç içe alanlara erişin
İç içe alanlara doğrudan sorgunuzdan kolayca erişmek için nokta gösterimi (.) kullanın. Örneğin, bu sorgu, önceki JSON verilerinde Konum özelliği altında Enlem ve Boylam koordinatlarını seçer. Nokta gösterimi, aşağıda gösterildiği gibi birden çok düzeyde gezinmek için kullanılabilir.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.Temperature,
    SensorReadings.SensorMetadata.Version
FROM input
```

Sonuç şudur:

|Deviceıd|Lat|Uzun|Sıcaklık|Sürüm|
|-|-|-|-|-|
|12345|47|122|80|1.2.45|


### <a name="select-all-properties"></a>Tüm özellikleri seçin
'*' joker kartını kullanarak iç içe kaydedilmiş kaydın tüm özelliklerini seçebilirsiniz. Aşağıdaki örneği inceleyin:

```SQL
SELECT
    DeviceID,
    Location.*
FROM input
```

Sonuç şudur:

|Deviceıd|Lat|Uzun|
|-|-|-|
|12345|47|122|


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Özellik adı değişken olduğunda iç içe olan alanlara erişin

Özellik adı değişkense [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getrecordpropertyvalue-azure-stream-analytics) işlevini kullanın. Bu, özellik adlarını zorlamadan dinamik sorgular oluşturmanızı sağlar.

Örneğin, örnek veri akışının her aygıt sensörü için eşikler içeren **başvuru verileriyle birleştirilmesi** gerektiğini düşünün. Bu tür referans verilerinin bir bölümü aşağıda gösterilmiştir.

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 85
},
{
    "DeviceId" : "12345",
    "SensorName" : "Humidity",
    "Value" : 65
}
```

Burada amaç, makalenin üst kısmından bu referans verisine örnek veri setimizi birleştirmek ve eşiğinin üzerindeki her sensör ölçüsü için bir olay çıkarmaktır. Bu, yukarıdaki tek olay, birleştirme sayesinde birden fazla sensör kendi eşiklerinin üzerindeyse birden fazla çıkış olayı oluşturabileceği anlamına gelir. Birleştirme olmadan benzer sonuçlar elde etmek için aşağıdaki bölüme bakın.

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName,
    "Alert : Sensor above threshold" AS AlertMessage
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

**GetRecordPropertyValue** *SensorReadings*özelliği seçer , hangi adı referans verilerinden gelen özellik adı eşleşir. Daha sonra *SensorReadings'in* ilişkili değeri ayıklanır.

Sonuç şudur:

|Deviceıd|Sensör Adı|AlertMessage|
|-|-|-|
|12345|Nem oranı|Uyarı : Eşik üstsensörü|

### <a name="convert-record-fields-into-separate-events"></a>Kayıt alanlarını ayrı olaylara dönüştürme

Kayıt alanlarını ayrı olaylara dönüştürmek için [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics) işlevi ile birlikte [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işlecikullanın.

Özgün örnek verilerle, özellikleri farklı olaylara ayıklamak için aşağıdaki sorgu kullanılabilir.

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

Sonuç şudur:

|Deviceıd|Sensör Adı|AlertMessage|
|-|-|-|
|12345|Sıcaklık|80|
|12345|Nem oranı|70|
|12345|ÖzelSensör01|5|
|12345|ÖzelSensor02|99|
|12345|SensörMeta veriler|[nesne Nesnesi]|

[WITH](https://docs.microsoft.com/stream-analytics-query/with-azure-stream-analytics)kullanarak, bu olayları farklı hedeflere yönlendirmek mümkündür:

```SQL
WITH Stage0 AS
(
    SELECT
        event.DeviceID,
        sensorReading.PropertyName,
        sensorReading.PropertyValue
    FROM input as event
    CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
)

SELECT DeviceID, PropertyValue AS Temperature INTO TemperatureOutput FROM Stage0 WHERE PropertyName = 'Temperature'
SELECT DeviceID, PropertyValue AS Humidity INTO HumidityOutput FROM Stage0 WHERE PropertyName = 'Humidity'
```

### <a name="parse-json-record-in-sql-reference-data"></a>SQL başvuru verilerinde Ayrışdırılan JSON kaydı
Azure SQL Veritabanı'nı işinizde başvuru verisi olarak kullanırken, JSON formatında veri içeren bir sütuna sahip olmak mümkündür. Aşağıda bir örnek gösterilmiştir.

|Deviceıd|Veri|
|-|-|
|12345|{"anahtar" : "value1"}|
|54321|{"anahtar" : "value2"}|

Basit bir JavaScript kullanıcı tanımlı işlev yazarak *Veri* sütunundaki JSON kaydını ayrıştırabilirsiniz.

```javascript
function parseJson(string) {
return JSON.parse(string);
}
```

Daha sonra, JSON kayıtlarınız alanlarına erişmek için aşağıda gösterildiği gibi Akış Analizi sorgunuzda bir adım oluşturabilirsiniz.

 ```SQL
 WITH parseJson as
 (
 SELECT DeviceID, udf.parseJson(sqlRefInput.Data) as metadata,
 FROM sqlRefInput
 )
 
 SELECT metadata.key
 INTO output
 FROM streamInput
 JOIN parseJson 
 ON streamInput.DeviceID = parseJson.DeviceID
```

## <a name="array-data-types"></a>Dizi veri türleri

Dizi veri türleri sıralı bir değer koleksiyonu. Dizi değerleri yle ilgili bazı tipik işlemler aşağıda ayrıntılı olarak açıklanmıştır. Bu [örnekler, GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics), ve [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci işlevlerini kullanın.

Burada tek bir olay bir örnektir. Her `CustomSensor03` `SensorMetadata` ikisi de ve tür **dizi**vardır:

```json
{
    "DeviceId" : "12345",
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "CustomSensor03": [12,-5,0]
     },
    "SensorMetadata":[
        {          
            "smKey":"Manufacturer",
            "smValue":"ABC"                
        },
        {
            "smKey":"Version",
            "smValue":"1.2.45"
        }
    ]
}
```

### <a name="working-with-a-specific-array-element"></a>Belirli bir dizi öğesi yle çalışma

Dizi öğesini belirli bir dizinte seçin (ilk dizi öğesini seçme):

```SQL
SELECT
    GetArrayElement(SensorReadings.CustomSensor03, 0) AS firstElement
FROM input
```

Sonuç şudur:

|firstElement|
|-|
|12|

### <a name="select-array-length"></a>Dizi uzunluğunu seçin

```SQL
SELECT
    GetArrayLength(SensorReadings.CustomSensor03) AS arrayLength
FROM input
```

Sonuç şudur:

|diziUzunluk|
|-|
|3|

### <a name="convert-array-elements-into-separate-events"></a>Dizi öğelerini ayrı olaylara dönüştürme

Tüm dizi öğesini tek tek olaylar olarak seçin. [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) yerleşik işlevi ile birlikte [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci, tüm dizi öğelerini tek tek olaylar olarak ayıklar:

```SQL
SELECT
    DeviceId,
    CustomSensor03Record.ArrayIndex,
    CustomSensor03Record.ArrayValue
FROM input
CROSS APPLY GetArrayElements(SensorReadings.CustomSensor03) AS CustomSensor03Record

```

Sonuç şudur:

|DeviceId|Arrayındex|Arrayvalue|
|-|-|-|
|12345|0|12|
|12345|1|-5|
|12345|2|0|

```SQL
SELECT   
    i.DeviceId, 
    SensorMetadataRecords.ArrayValue.smKey as smKey,
    SensorMetadataRecords.ArrayValue.smValue as smValue
FROM input i
CROSS APPLY GetArrayElements(SensorMetadata) AS SensorMetadataRecords
 ```
 
Sonuç şudur:

|DeviceId|smKey|smValue|
|-|-|-|
|12345|Üretici|ABC|
|12345|Sürüm|1.2.45|

Ayıklanan alanların sütunlarda görünmesi gerekiyorsa, [JOIN](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics) işlemine ek olarak [WITH](https://docs.microsoft.com/stream-analytics-query/with-azure-stream-analytics) sözdizimini kullanarak veri kümesini döndürmek mümkündür. Bu birleştirme, yinelemeyi engelleyen bir [zaman sınırı](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics#BKMK_DateDiff) koşulu gerektirir:

```SQL
WITH DynamicCTE AS (
    SELECT   
        i.DeviceId,
        SensorMetadataRecords.ArrayValue.smKey as smKey,
        SensorMetadataRecords.ArrayValue.smValue as smValue
    FROM input i
    CROSS APPLY GetArrayElements(SensorMetadata) AS SensorMetadataRecords 
)

SELECT
    i.DeviceId,
    i.Location.*,
    V.smValue AS 'smVersion',
    M.smValue AS 'smManufacturer'
FROM input i
LEFT JOIN DynamicCTE V ON V.smKey = 'Version' and V.DeviceId = i.DeviceId AND DATEDIFF(minute,i,V) BETWEEN 0 AND 0 
LEFT JOIN DynamicCTE M ON M.smKey = 'Manufacturer' and M.DeviceId = i.DeviceId AND DATEDIFF(minute,i,M) BETWEEN 0 AND 0
```

Sonuç şudur:

|DeviceId|Lat|Uzun|smVersion|smÜretici|
|-|-|-|-|-|
|12345|47|122|1.2.45|ABC|

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Akış Analizinde Veri Türleri](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
