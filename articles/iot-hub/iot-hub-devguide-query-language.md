---
title: Azure IoT Hub sorgu dilini anlayın | Microsoft Docs
description: Geliştirici Kılavuzu-IoT Hub 'ınızdaki cihaz/modül TWINS ve işleri hakkında bilgi almak için kullanılan SQL benzeri IoT Hub sorgu dilinin açıklaması.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: robinsh
ms.openlocfilehash: b224de96f6b6baedc3b57e0245a4c4e8748576b4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79271141"
---
# <a name="iot-hub-query-language-for-device-and-module-twins-jobs-and-message-routing"></a>Cihaz ve modül TWINS, işler ve ileti yönlendirme için sorgu dili IoT Hub

IoT Hub, [cihaz TWINS](iot-hub-devguide-device-twins.md), [Modül TWINS](iot-hub-devguide-module-twins.md), [işler](iot-hub-devguide-jobs.md)ve [ileti yönlendirme](iot-hub-devguide-messages-d2c.md)ile ilgili bilgileri almak için güçlü bir SQL benzeri dil sağlar. Bu makalede şunları sunulmaktadır:

* IoT Hub sorgu dilinin ana özelliklerine giriş ve
* Dilin ayrıntılı açıklaması. İleti yönlendirme için sorgu dili hakkındaki ayrıntılar için bkz. [ileti yönlendirmesinde sorgular](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="device-and-module-twin-queries"></a>Cihaz ve modül ikizi sorguları

[Cihaz TWINS](iot-hub-devguide-device-twins.md) ve [Modül TWINS](iot-hub-devguide-module-twins.md) , hem Etiketler hem de özellikler olarak rastgele JSON nesneleri içerebilir. IoT Hub, tüm ikizi bilgilerini içeren tek bir JSON belgesi olarak Device TWINS ve modül TWINS 'i sorgulamanızı sağlar.

Örneğin, IoT Hub cihazlarınızın aşağıdaki yapıya sahip olduğunu varsayın (modül ikizi, yalnızca ek bir ModuleID ile benzerdir):

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "Disconnected",
    "lastActivityTime": "0001-01-01T00:00:00",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "version": 2,
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

### <a name="device-twin-queries"></a>Cihaz ikizi sorguları

IoT Hub cihaz TWINS 'i **cihazlar**adlı bir belge koleksiyonu olarak kullanıma sunar. Örneğin, aşağıdaki sorgu tüm cihaz TWINS kümesini alır:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT SDK 'ları](iot-hub-devguide-sdks.md) büyük sonuçların sayfalamayı destekler.

IoT Hub, rastgele koşullara göre cihaz ikizlerini filtrelemesini almanızı sağlar. Örneğin, **Location. Region** etiketinin **US** olarak ayarlandığı cihaz TWINS 'i almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

Boole işleçleri ve aritmetik karşılaştırmalar de desteklenir. Örneğin, ABD 'de bulunan ve her dakikadan daha az telemetri gönderecek şekilde yapılandırılmış cihaz ikizlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

Bir kolaylık olması halinde, dizi sabitlerini de **ın** ve **ın** (Not ın) işleçleriyle kullanmak da mümkündür. Örneğin, WiFi veya kablolu bağlantıyı rapor eden cihaz TWINS 'sini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

Belirli bir özelliği içeren tüm cihaz TWINS 'i tanımlamak genellikle gereklidir. IoT Hub, bu amaçla `is_defined()` işlevi destekler. Örneğin, `connectivity` özelliğini tanımlayan cihaz TWINS 'sini almak için aşağıdaki sorguyu kullanın:

```SQL
SELECT * FROM devices
  WHERE is_defined(properties.reported.connectivity)
```

Filtreleme yeteneklerinin tam başvurusu için [WHERE yan tümcesi](iot-hub-devguide-query-language.md#where-clause) bölümüne bakın.

Gruplandırma ve toplamalar de desteklenir. Örneğin, her telemetri yapılandırma durumundaki cihaz sayısını bulmak için aşağıdaki sorguyu kullanın:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
  FROM devices
  GROUP BY properties.reported.telemetryConfig.status
```

Bu gruplandırma sorgusu, aşağıdaki örneğe benzer bir sonuç döndürür:

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Bu örnekte, üç cihaz başarılı bir yapılandırma raporladı, iki yapılandırma hala yapılandırmayı uyguluyor ve bir hata bildirdi.

Projeksiyon sorguları, geliştiricilerin yalnızca ilgilendikleri özellikleri döndürmesini sağlar. Örneğin, tüm bağlantısı kesilen cihazların son etkinlik saatini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT LastActivityTime FROM devices WHERE status = 'enabled'
```

### <a name="module-twin-queries"></a>Module ikizi sorguları

Modül TWINS 'de sorgulama, cihaz ikiklikleri, farklı bir koleksiyon/ad alanı kullanılarak sorgulanmasına benzer. cihazlar yerine **cihazlar. modüller** **' de**sorgulama yapabilirsiniz:

```sql
SELECT * FROM devices.modules
```

Cihazlar ve cihazlar. modüller koleksiyonları arasında birleşime izin vermedik. Cihazlarda modül genelindeki modülleri sorgulamak istiyorsanız, etiketleri temel alarak yapabilirsiniz. Bu sorgu, tarama durumuna sahip tüm cihazlarda tüm modül TWINS 'i döndürür:

```sql
SELECT * FROM devices.modules WHERE properties.reported.status = 'scanning'
```

Bu sorgu tüm modül TWINS 'i tarama durumuyla döndürür, ancak yalnızca belirtilen cihaz alt kümesinde:

```sql
SELECT * FROM devices.modules
  WHERE properties.reported.status = 'scanning'
  AND deviceId IN ['device1', 'device2']
```

### <a name="c-example"></a>C#örneğinde

Sorgu işlevi, **registrymanager** sınıfında [ C# hizmet SDK 'sı](iot-hub-devguide-sdks.md) tarafından sunulur.

Basit bir sorgu örneği aşağıda verilmiştir:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

**Sorgu** nesnesi, bir sayfa boyutu (100 'e kadar) ile oluşturulur. Daha sonra, **Getnextastwınasync** yöntemleri birden çok kez çağırarak birden çok sayfa alınır.

Sorgu nesnesi, sorgu için gereken seri kaldırma seçeneğine bağlı olarak birden çok **sonraki** değeri kullanıma sunar. Örneğin, cihaz ikizi veya iş nesneleri ya da projeksiyonlar kullanılırken düz JSON.

### <a name="nodejs-example"></a>Node. js örneği

Sorgu işlevselliği, **kayıt defteri** nesnesindeki [Node. js için Azure IoT hizmeti SDK 'sı](iot-hub-devguide-sdks.md) tarafından sunulur.

Basit bir sorgu örneği aşağıda verilmiştir:

```javascript
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

**Sorgu** nesnesi, bir sayfa boyutu (100 'e kadar) ile oluşturulur. Ardından, **Nextastwin** yöntemi birden çok kez çağırarak birden çok sayfa alınır.

Sorgu nesnesi, sorgu için gereken seri kaldırma seçeneğine bağlı olarak birden çok **sonraki** değeri kullanıma sunar. Örneğin, cihaz ikizi veya iş nesneleri ya da projeksiyonlar kullanılırken düz JSON.

### <a name="limitations"></a>Sınırlamalar

> [!IMPORTANT]
> Sorgu sonuçları, cihaz iksındaki en son değerlere göre birkaç dakikalık gecikmeye neden olabilir. Bağımsız cihaz TWINS 'i KIMLIĞE göre sorgularken [Get ikizi REST API](https://docs.microsoft.com/rest/api/iothub/service/gettwin)kullanın. Bu API her zaman en son değerleri döndürür ve daha yüksek azaltma sınırlarına sahiptir. REST API doğrudan verebilir veya [Azure IoT Hub hizmeti SDK](iot-hub-devguide-sdks.md#azure-iot-hub-service-sdks)'larından birindeki denk işlevselliği kullanabilirsiniz.

Şu anda, karşılaştırmalar yalnızca ilkel türler arasında desteklenir (nesne yok), örneğin `... WHERE properties.desired.config = properties.reported.config` yalnızca bu özelliklerde ilkel değerler varsa desteklenir.

## <a name="get-started-with-jobs-queries"></a>İşleri sorgular ile çalışmaya başlama

[İşler](iot-hub-devguide-jobs.md) , cihaz kümelerinde işlemleri yürütmek için bir yol sağlar. Her cihaz ikizi, **işler**adlı bir koleksiyonda yer aldığı işlerin bilgilerini içerir.

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Şu anda, bu koleksiyon IoT Hub sorgu dilinde **Devices.Jobs** olarak sorgulanabilir.

> [!IMPORTANT]
> Şu anda, Device TWINS sorgulanırken işler özelliği hiçbir zaman döndürülmez. Diğer bir deyişle, ' FROM devices ' içeren sorgular. İşler özelliğine yalnızca `FROM devices.jobs`kullanarak doğrudan sorgularla erişilebilir.
>
>

Örneğin, tek bir cihazı etkileyen tüm işleri (geçmişte ve zamanlanan) almak için aşağıdaki sorguyu kullanabilirsiniz:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
```

Bu sorgunun döndürülen her iş için cihaza özgü durumu (ve muhtemelen doğrudan yöntem yanıtı) nasıl sağladığını aklınızda bulabilirsiniz.

Ayrıca, **Devices.Jobs** koleksiyonundaki tüm nesne özelliklerinde rastgele Boole koşullara göre filtrelemek mümkündür.

Örneğin, belirli bir cihaz için 2016 Eylül 'tan sonra oluşturulan tüm tamamlanmış Device ikizi Update işlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

Tek bir işin cihaz başına sonuçlarını da alabilirsiniz.

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Sınırlamalar

Şu anda, **Devices.Jobs** üzerindeki sorgular şunları desteklemez:

* Bu nedenle, yalnızca `SELECT *` mümkündür.
* İş özelliklerine ek olarak cihaz ikizi başvuran koşullar (yukarıdaki bölüme bakın).
* Count, AVG, group by gibi toplamalar gerçekleştiriliyor.

## <a name="basics-of-an-iot-hub-query"></a>IoT Hub sorgusunun temelleri

Her IoT Hub sorgu SELECT ve FROM yan tümcelerinden, isteğe bağlı WHERE ve GROUP BY yan tümceleriyle oluşur. Her sorgu, JSON belgelerinin bir koleksiyonunda çalıştırılır, örneğin cihaz ikikesi. FROM yan tümcesi, (**cihazlar**, **cihazlar. modüller**veya **Devices.Jobs**) üzerinde tekrarlandırılmış belge koleksiyonunu belirtir. Sonra WHERE yan tümcesindeki filtre uygulanır. Toplamalar ile, bu adımın sonuçları GROUP BY yan tümcesinde belirtilen şekilde gruplandırılır. Her grup için, SELECT yan tümcesinde belirtilen şekilde bir satır oluşturulur.

```sql
SELECT <select_list>
  FROM <from_specification>
  [WHERE <filter_condition>]
  [GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM yan tümcesi

**FROM < from_specification >** yan tümcesi yalnızca üç değeri kabul edebilir: **cihazlardan** cihaz ikizlerini sorgular **. modüller, modülleri** sorgulamak için veya **Devices.Jobs 'den** sorgu işi-cihaz başına ayrıntılar.

## <a name="where-clause"></a>WHERE tümcesi

**Where < filter_condition >** yan tümcesi isteğe bağlıdır. Bu, FROM koleksiyonundaki JSON belgelerinin sonucun bir parçası olarak dahil edilmesini sağlamak için karşılaması gereken bir veya daha fazla koşulu belirtir. Herhangi bir JSON belgesi, sonuca eklenmek için belirtilen koşulları "true" olarak değerlendirmelidir.

İzin verilen koşullar, bölüm [ifadelerinde ve koşullarında](iot-hub-devguide-query-language.md#expressions-and-conditions)açıklanmıştır.

## <a name="select-clause"></a>SELECT yan tümcesi

**SELECT < select_list >** zorunludur ve sorgudan alınan değerleri belirtir. Yeni JSON nesneleri oluşturmak için kullanılacak JSON değerlerini belirtir.
IÇINDEKI koleksiyonun filtrelenmiş (ve isteğe bağlı olarak gruplanmış) alt kümesinin her öğesi için, projeksiyon aşaması yeni bir JSON nesnesi oluşturur. Bu nesne SELECT yan tümcesinde belirtilen değerlerle oluşturulur.

SELECT yan tümcesinin dilbilgisini aşağıda verilmiştir:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

**Attribute_name** , from koleksiyonundaki JSON belgesinin herhangi bir özelliğine başvurur. SELECT yan tümcelerinin bazı örnekleri, Device ikizi sorguları ile çalışmaya başlama bölümünde bulunabilir.

Şu anda, **Select*** öğesinden farklı seçim yan tümceleri yalnızca cihaz ikslarındaki toplu sorgularda desteklenir.

## <a name="group-by-clause"></a>GROUP BY yan tümcesi

**GROUP BY < group_specification >** yan tümcesi WHERE yan tümcesinde belirtilen filtreden sonra ve Select içinde belirtilen projeksiyon öncesinde yürütülen isteğe bağlı bir adımdır. Bir özniteliğin değerine göre belgeleri gruplandırır. Bu gruplar, SELECT yan tümcesinde belirtilen şekilde toplanmış değerler oluşturmak için kullanılır.

GROUP BY kullanan bir sorgu örneği:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

GROUP BY için biçimsel sözdizimi şöyledir:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

**Attribute_name** , from koleksiyonundaki JSON belgesinin herhangi bir özelliğine başvurur.

Şu anda GROUP BY yan tümcesi yalnızca cihaz ikizleri sorgulanırken desteklenir.

> [!IMPORTANT]
> `group` terimi Şu anda sorgularda özel bir anahtar sözcük olarak kabul edilir. Büyük/küçük harf olarak `group`, özellik adı olarak kullanılır, örneğin, `SELECT * FROM devices WHERE tags.[[group]].name = 'some_value'`hataları önlemek için çift ayraçla çevrelemeyi düşünün.
>

## <a name="expressions-and-conditions"></a>İfadeler ve koşullar

Yüksek düzeyde, bir *ifade*:

* Bir JSON türü örneği (Boolean, sayı, dize, dizi veya nesne gibi) olarak değerlendirilir.
* , Yerleşik işleçler ve işlevler kullanılarak cihaz JSON belgesinden ve sabitlerinden gelen veriler işlenerek tanımlanır.

*Koşullar* bir Boole değeri değerlendiren ifadelerdir. Boolean **true** değerinden farklı olan herhangi bir sabit **yanlış**olarak değerlendirilir. Bu kural **null**, **tanımsız**, herhangi bir nesne veya dizi örneği, herhangi bir dize **ve Boole değeri**içerir.

İfadelerin sözdizimi şöyledir:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

İfadeler sözdiziminde her bir simgenin ne olduğunu anlamak için aşağıdaki tabloya bakın:

| Sembol | Tanım |
| --- | --- |
| attribute_name | **From** koleksiyonundaki JSON belgesinin herhangi bir özelliği. |
| binary_operator | [İşleçler](#operators) bölümünde listelenen herhangi bir ikili işleç. |
| function_name| [İşlevler](#functions) bölümünde listelenen herhangi bir işlev. |
| decimal_literal |Ondalık gösteriminde ifade edilen bir float. |
| hexadecimal_literal |' 0x ' dizesinin ardından onaltılık basamak dizesi tarafından ifade edilen bir sayı. |
| string_literal |Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış dizileri tarafından temsil edilen Unicode dizelerdir. Dize sabit değerleri tek tırnak veya çift tırnak içine alınır. İzin verilen kaçış: `\'`, `\"`, `\\`, 4 onaltılık basamak tarafından tanımlanan Unicode karakterler için `\uXXXX`. |

### <a name="operators"></a>İşleçler

Aşağıdaki işleçler desteklenir:

| Ailesi | İşleçler |
| --- | --- |
| Aritmetik |+, -, *, /, % |
| Mantıksal |VE, VEYA DEĞİL |
| Karşılaştırma |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>İşlevler

TWINS ve işleri sorgularken yalnızca desteklenen işlev şunlardır:

| İşlev | Açıklama |
| -------- | ----------- |
| IS_DEFINED (özellik) | Özelliğin bir değer atandığını (`null`dahil) belirten bir Boole değeri döndürür. |

Rotalar koşullarında aşağıdaki matematik işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| ABS (x) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| EXP (x) | Belirtilen sayısal ifadenin üstel değerini döndürür (e ^ x). |
| Güç (x, y) | Belirtilen bir ifadenin belirtilen kuvvetinin değerini döndürür (x ^ y).|
| KARE (x) | Belirtilen bir sayısal değer karesini döndürür. |
| TAVAN (x) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür. |
| KAT (x) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür. |
| IMZALA (x) | (+ 1) pozitif, sıfır (0) veya eksi (-1) belirtilen sayısal ifade döndürür.|
| SQRT (x) | Belirtilen sayısal değerinin kare kökünü döndürür. |

Yollar koşullarında, aşağıdaki tür denetimi ve atama işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| AS_NUMBER | Giriş dizesini bir sayıya dönüştürür. Giriş bir sayı ise `noop`; dize bir sayıyı temsil etmediği `Undefined`.|
| IS_ARRAY | Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_BOOL | Belirtilen ifade türünü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_DEFINED | Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür. |
| IS_NULL | Belirtilen ifadenin türü null olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_NUMBER | Belirtilen ifade türünü bir sayı olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_OBJECT | Belirtilen ifade türünü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_PRIMITIVE | Belirtilen ifadenin türünün bir ilkel öğe (dize, Boole, sayısal veya `null`) olduğunu gösteren bir Boole değeri döndürür. |
| IS_STRING | Belirtilen ifadenin türü dize olup olmadığını gösteren bir Boole değeri döndürür. |

Yollar koşullarında aşağıdaki dize işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| CONCAT (x, y,...) | İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür. |
| Uzunluk (x) | Belirtilen dize ifadesinin karakter sayısını döndürür.|
| DAHA düşük (x) | Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| UPPER (x) | Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| Alt DIZE (dize, başlangıç [, uzunluk]) | Belirtilen karakterin sıfır tabanlı konumunda başlayan bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dizenin sonuna kadar devam eder. |
| INDEX_OF (dize, parça) | İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa, ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize.|
| STARTS_WITH (x, y) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır. |
| ENDS_WITH (x, y) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer. |
| IÇERIR (x, y) | Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT SDK](iot-hub-devguide-sdks.md)'larını kullanarak uygulamalarınızda sorguları yürütmeyi öğrenin.