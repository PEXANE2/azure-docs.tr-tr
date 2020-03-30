---
title: Azure IoT Hub sorgu dilini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT hub'ınızdan aygıt/modül ikizleri ve işleri hakkında bilgi almak için kullanılan SQL benzeri IoT Hub sorgu dilinin açıklaması.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: robinsh
ms.openlocfilehash: ad8b4b39e582d10c2a3b6003bfa07138f4697b71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499197"
---
# <a name="iot-hub-query-language-for-device-and-module-twins-jobs-and-message-routing"></a>Cihaz ve modül ikizleri, işler ve mesaj yönlendirmesi için IoT Hub sorgu dili

IoT [Hub, aygıt ikizleri,](iot-hub-devguide-device-twins.md)modül [ikizleri,](iot-hub-devguide-module-twins.md) [işleri](iot-hub-devguide-jobs.md)ve ileti [yönlendirmeile](iot-hub-devguide-messages-d2c.md)ilgili bilgi almak için güçlü bir SQL benzeri dil sağlar. Bu makalede sunar:

* IoT Hub sorgu dilinin ana özelliklerine giriş ve
* Dilin ayrıntılı açıklaması. İleti yönlendirme için sorgu dili yle ilgili ayrıntılar için [ileti yönlendirmedeki sorgulara](../iot-hub/iot-hub-devguide-routing-query-syntax.md)bakın.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="device-and-module-twin-queries"></a>Cihaz ve modül ikiz sorguları

[Aygıt ikizleri](iot-hub-devguide-device-twins.md) ve [modül ikizleri](iot-hub-devguide-module-twins.md) hem etiketleri hem de özellikleri olarak rasgele JSON nesneleri içerebilir. IoT Hub, tüm ikiz bilgilerini içeren tek bir JSON belgesi olarak aygıt ikizleri ve modül ikizleri sorgulamanızı sağlar.

Örneğin, IoT hub aygıtı ikizlerinizin aşağıdaki yapıya sahip olduğunu varsayalım (modül ikizi sadece ek bir moduleId ile benzer olacaktır):

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

### <a name="device-twin-queries"></a>Aygıt ikiz sorguları

IoT Hub aygıt ikizleri **aygıt**lar olarak adlandırılan bir belge koleksiyonu olarak ortaya çıkarır. Örneğin, aşağıdaki sorgu aygıt ikizlerinin tüm kümesini alır:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT SDK'lar](iot-hub-devguide-sdks.md) büyük sonuçların çağrılmalarını destekler.

IoT Hub, rasgele koşullarla filtreleme sağlayan aygıt ikizlerini almanızı sağlar. Örneğin, **konum.bölge** etiketinin **ABD** olarak ayarlandığı aygıt ikizlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

Boolean operatörleri ve aritmetik karşılaştırmalar da desteklenir. Örneğin, ABD'de bulunan ve her dakikadan daha az telemetri gönderecek şekilde yapılandırılan aygıt ikizlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

Kolaylık sağlamak için, **IN** ve **NIN** (içinde değil) işleçleri ile dizi sabitleri kullanmak da mümkündür. Örneğin, WiFi veya kablolu bağlantı bildiren aygıt ikizlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

Genellikle belirli bir özellik içeren tüm cihaz ikizleri tanımlamak için gereklidir. IoT Hub bu `is_defined()` amaç için işlevi destekler. Örneğin, `connectivity` özelliği tanımlayan aygıt ikizlerini almak için aşağıdaki sorguyu kullanın:

```SQL
SELECT * FROM devices
  WHERE is_defined(properties.reported.connectivity)
```

Filtreleme özelliklerinin tam başvurusu için [WHERE yan tümcesi](iot-hub-devguide-query-language.md#where-clause) bölümüne bakın.

Gruplandırma ve toplamalar da desteklenir. Örneğin, her telemetri yapılandırma durumundaki aygıt sayısını bulmak için aşağıdaki sorguyu kullanın:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
  FROM devices
  GROUP BY properties.reported.telemetryConfig.status
```

Bu gruplandırma sorgusu aşağıdaki örneğe benzer bir sonuç döndürecek:

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

Bu örnekte, üç aygıt başarılı yapılandırma rapor, iki hala yapılandırma uygulamakta ve bir hata bildirdi.

Projeksiyon sorguları, geliştiricilerin yalnızca önem verdikleri özellikleri döndürmesine olanak tanır. Örneğin, bağlantısı kesilen tüm aygıtların son etkinlik süresini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT LastActivityTime FROM devices WHERE status = 'enabled'
```

### <a name="module-twin-queries"></a>Modül ikiz sorguları

Modül ikizleri üzerinde sorgulama, aygıt ikizleri üzerinde sorgulama benzer, ancak farklı bir koleksiyon / ad alanı kullanarak; **cihazlardan**değil, **devices.modules**sorgu:

```sql
SELECT * FROM devices.modules
```

Cihazlar ve cihazlar.modül koleksiyonları arasında birleştirmeye izin vermiyoruz. Modül ikizlerini aygıtlar arasında sorgulamak istiyorsanız, bunu etiketlere göre yaparsınız. Bu sorgu, tarama durumuna sahip tüm aygıtlar arasındaki tüm modül ikizlerini döndürecektir:

```sql
SELECT * FROM devices.modules WHERE properties.reported.status = 'scanning'
```

Bu sorgu, yalnızca aygıtların belirtilen alt kümesinde, tarama durumuna sahip tüm modül ikizlerini döndürecektir:

```sql
SELECT * FROM devices.modules
  WHERE properties.reported.status = 'scanning'
  AND deviceId IN ['device1', 'device2']
```

### <a name="c-example"></a>C# örneği

Sorgu işlevi **RegistryManager** sınıfında [C# hizmeti SDK](iot-hub-devguide-sdks.md) tarafından ortaya çıkarır.

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

**Sorgu** nesnesi bir sayfa boyutu (en fazla 100) ile anında verilir. Daha sonra **getNextAsTwinAsync** yöntemlerini birden çok kez arayarak birden çok sayfa alınır.

Sorgu nesnesi, sorgunun gerektirdiği deserialization seçeneğine bağlı olarak birden çok **Sonraki** değeri gösterir. Örneğin, aygıt ikizi veya iş nesneleri veya projeksiyonları kullanırken düz JSON.

### <a name="nodejs-example"></a>Düğüm.js örneği

Sorgu işlevi, **Kayıt Defteri** nesnesindeki [Node.js için Azure IoT hizmeti SDK](iot-hub-devguide-sdks.md) tarafından ortaya çıkarır.

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

**Sorgu** nesnesi bir sayfa boyutu (en fazla 100) ile anında verilir. Daha sonra **nextAsTwin** yöntemini birden çok kez arayarak birden çok sayfa alınır.

Sorgu nesnesi, sorgunun gerektirdiği deserialization seçeneğine bağlı olarak birden çok **Sonraki** değeri gösterir. Örneğin, aygıt ikizi veya iş nesneleri veya projeksiyonları kullanırken düz JSON.

### <a name="limitations"></a>Sınırlamalar

> [!IMPORTANT]
> Sorgu sonuçları, aygıt ikizlerinde en son değerlere göre birkaç dakika gecikmeye neden olabilir. Tek tek aygıt ikizlerini kimlikle sorgulıyorsanız, [ikiz REST API'sını](https://docs.microsoft.com/rest/api/iothub/service/twin/getdevicetwin)kullanın. Bu API her zaman en son değerleri döndürür ve daha yüksek azaltma limitlerine sahiptir. REST API'sini doğrudan verebilir veya [Azure IoT Hub Hizmeti SDK'larından](iot-hub-devguide-sdks.md#azure-iot-hub-service-sdks)birinde eşdeğer işlevselliği kullanabilirsiniz.

Şu anda karşılaştırmalar yalnızca ilkel türler (nesne yok) arasında desteklenir, örneğin `... WHERE properties.desired.config = properties.reported.config` yalnızca bu özelliklerin ilkel değerleri varsa desteklenir.

## <a name="get-started-with-jobs-queries"></a>İş sorgularıyla başlayın

[İşler,](iot-hub-devguide-jobs.md) aygıt kümelerinde işlemleri yürütmek için bir yol sağlar. Her aygıt ikizi, **iş**adı verilen bir koleksiyonda parçası olduğu işlerin bilgilerini içerir.

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

Şu anda, bu koleksiyon IoT Hub sorgu dilinde **devices.jobs** olarak sorgulanabilir.

> [!IMPORTANT]
> Şu anda, aygıt ikizlerini sorgularken işler özelliği hiçbir zaman döndürülmedi. Diğer bir şey, 'FROM aygıtları' içeren sorgular. İşler özelliğine yalnızca doğrudan sorgularla `FROM devices.jobs`erişilebilir.
>
>

Örneğin, tek bir aygıtı etkileyen tüm işleri (geçmiş ve zamanlanmış) almak için aşağıdaki sorguyu kullanabilirsiniz:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
```

Bu sorgunun döndürülen her işin aygıta özgü durumunu (ve büyük olasılıkla doğrudan yöntem yanıtını) nasıl sağladığını unutmayın.

**devices.jobs** koleksiyonundaki tüm nesne özelliklerine rasgele Boolean koşullarıyla filtre uygulayabilirsiniz.

Örneğin, belirli bir aygıt için Eylül 2016'dan sonra oluşturulan tamamlanmış tüm aygıt ikiz güncelleştirme işlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

Ayrıca, tek bir işin aygıt başına sonuçlarını da alabilirsiniz.

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Sınırlamalar

Şu anda, **devices.jobs** sorguları desteklenmez:

* Projeksiyonlar, bu `SELECT *` nedenle sadece mümkündür.
* İş özelliklerine ek olarak aygıt ikizine başvuran koşullar (önceki bölüme bakın).
* Count, avg, group by gibi toplamaları gerçekleştirir.

## <a name="basics-of-an-iot-hub-query"></a>IoT Hub sorgusunun temelleri

Her IoT Hub sorgusu, isteğe bağlı WHERE ve GROUP BY yan tümceleriyle SELECT ve FROM yan tümcelerinden oluşur. Her sorgu JSON belgelerinin bir koleksiyon, örneğin cihaz ikizler çalıştırılır. FROM yan tümcesi belge koleksiyonunun **(cihazlar,** **devices.modules**veya **devices.jobs)** üzerinde titreşeceğini gösterir. Daha sonra WHERE yan tümcesindeki filtre uygulanır. Toplamalarda, bu adımın sonuçları GROUP BY yan tümcesinde belirtildiği şekilde gruplandırılır. Her grup için SELECT yan tümcesinde belirtildiği gibi bir satır oluşturulur.

```sql
SELECT <select_list>
  FROM <from_specification>
  [WHERE <filter_condition>]
  [GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM yan tümcesi

**FROM <from_specification>** yan tümcesi yalnızca üç değer üstlenebilir: **aygıtlardan** aygıt ikizlerini sorguya, **FROM devices.modules'den** sorgu modülü ne kadar, veya **FROM devices.jobs'dan** aygıt başına iş ayrıntılarını sorgulaya.

## <a name="where-clause"></a>WHERE yan tümcesi

**WHERE <filter_condition>** maddesi isteğe bağlıdır. FROM koleksiyonundaki JSON belgelerinin sonucun bir parçası olarak dahil edilebilmek için karşılanması gereken bir veya daha fazla koşulu belirtir. Herhangi bir JSON belgesi, sonuca dahil edilebilmek için belirtilen koşulları "doğru" olarak değerlendirmelidir.

İzin verilen koşullar bölüm [İfadeler ve koşullar](iot-hub-devguide-query-language.md#expressions-and-conditions)açıklanmıştır.

## <a name="select-clause"></a>SELECT yan tümcesi

**SELECT <select_list>** zorunludur ve sorgudan hangi değerlerin alınıp alınacaklarını belirtir. Yeni JSON nesneleri oluşturmak için kullanılacak JSON değerlerini belirtir.
FROM koleksiyonunun filtrelenmiş (ve isteğe bağlı olarak gruplandırılan) alt kümesinin her öğesi için projeksiyon aşaması yeni bir JSON nesnesi oluşturur. Bu nesne SELECT yan tümcesinde belirtilen değerlerle oluşturulur.

SELECT yan tümcesinin dilbilgisi aşağıdadır:

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

**Attribute_name,** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğine atıfta bulunur. SELECT yan tümcelerine bazı örnekler aygıt ikiz sorguları ile başlar bölümünde bulunabilir.

Şu anda SELECT ***'den**farklı seçim yan tümceleri yalnızca aygıt ikizlerinde toplu sorgularda desteklenir.

## <a name="group-by-clause"></a>GROUP BY yan tümcesi

**GROUP BY <group_specification>** yan tümcesi, WHERE yan tümcesinde belirtilen filtreden sonra ve SELECT'te belirtilen projeksiyondan önce uygulayan isteğe bağlı bir adımdır. Belgeleri bir öznitelik değerine göre gruplar. Bu gruplar SELECT yan tümcesinde belirtildiği gibi toplanan değerleri oluşturmak için kullanılır.

GROUP BY'yi kullanan bir sorgu örneği:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

GROUP BY için resmi sözdizimi:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

**Attribute_name,** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğine atıfta bulunur.

Şu anda, GROUP BY yan tümcesi yalnızca aygıt ikizlerini sorgularken desteklenir.

> [!IMPORTANT]
> Terim `group` şu anda sorgularda özel bir anahtar kelime olarak kabul edilir. Durumda, mülk `group` adınız olarak kullanırsanız, hataları önlemek için onu çift parantezle `SELECT * FROM devices WHERE tags.[[group]].name = 'some_value'`çevrelemeyi düşünün, örneğin.
>

## <a name="expressions-and-conditions"></a>İfadeler ve koşullar

Yüksek düzeyde, bir *ifade:*

* JSON türü (Boolean, sayı, dize, dizi veya nesne gibi) bir örneğine değerlendirir.
* JSON belgesive sabitlerden gelen verilerin yerleşik işleçler ve işlevler kullanılarak manipüle edilmesiyle tanımlanır.

*Koşullar* boolean için değerlendirmek ifadelerdir. Boolean **gerçek** herhangi bir sabit farklı **yanlış**olarak kabul edilir. Bu kural **null,** **tanımlanmamış,** herhangi bir nesne veya dizi örneği, herhangi bir dize ve Boolean **yanlış**içerir.

İfadeler için sözdizimi:

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

Sözdizimi ifadelerindeki her sembolün ne anlama geldiğini anlamak için aşağıdaki tabloya bakın:

| Sembol | Tanım |
| --- | --- |
| Öznitelik_adý | **FROM** koleksiyonundaki JSON belgesinin herhangi bir özelliği. |
| binary_operator | [Operatörler](#operators) bölümünde listelenen herhangi bir ikili işleç. |
| function_name| [Fonksiyonlar](#functions) bölümünde listelenen herhangi bir işlev. |
| decimal_literal |Ondalık gösterimle ifade edilen bir float. |
| hexadecimal_literal |'0x' dizesi tarafından ifade edilen bir sayı ve ardından bir hexadecimal basamak dizesi. |
| string_literal |String literals, sıfır veya daha fazla Unicode karakter veya kaçış dizileri dizisi yle temsil edilen Unicode dizeleridir. String literals tek tırnak veya çift tırnak içinde kapalıdır. İzin verilen `\'`kaçışlar: `\uXXXX` , , , `\"` `\\`4 hexadecimal basamak ile tanımlanan Unicode karakterleri için. |

### <a name="operators"></a>İşleçler

Aşağıdaki işleçler desteklenir:

| Family (Aile) | İşleçler |
| --- | --- |
| Aritmetik |+, -, *, /, % |
| Mantıksal |VE, YA DA, DEĞIL |
| Karşılaştırma |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>İşlevler

İkizleri ve işleri sorgularken desteklenen tek işlev şudur:

| İşlev | Açıklama |
| -------- | ----------- |
| IS_DEFINED (özellik) | Özellik bir değer atanmış olup olmadığını belirten bir Boolean döndürür (dahil). `null` |

Rota koşullarında aşağıdaki matematik işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| ABS(x) | Belirtilen sayısal ifadenin mutlak (pozitif) değerini verir. |
| EXP(x) | Belirtilen sayısal ifadenin (e^x) üstel değerini verir. |
| GÜÇ(x,y) | Belirtilen ifadenin değerini belirtilen güce (x^y) verir.|
| KARE(x)    | Belirtilen sayısal değerin karesini verir. |
| TAVAN(x) | Belirtilen sayısal ifadeden daha büyük veya eşit olan en küçük tümseci değerini verir. |
| KAT(x) | Belirtilen sayısal ifadeden daha az veya eşit olan en büyük tümseci döndürür. |
| İşaret(x) | Belirtilen sayısal ifadenin pozitif (+1), sıfır (0) veya negatif (-1) işaretini verir.|
| SQRT(x) | Belirtilen sayısal değerin kare kökünü verir. |

Rota koşullarında, aşağıdaki tip kontrol ve döküm fonksiyonları desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| AS_NUMBER | Giriş dizesini bir sayıya dönüştürür. `noop`giriş bir sayı ise; `Undefined` dize bir sayıtemsil etmiyorsa.|
| IS_ARRAY | Belirtilen ifade nin türü bir dizi olup olmadığını belirten bir Boolean değeri döndürür. |
| IS_BOOL | Belirtilen ifadenin türü boolean olup olmadığını belirten bir Boolean değeri döndürür. |
| IS_DEFINED | Özellik bir değer atanmış olup olmadığını belirten bir Boolean döndürür. |
| IS_NULL | Belirtilen ifadenin türünün null olup olmadığını belirten bir Boolean değeri döndürür. |
| IS_NUMBER | Belirtilen ifadenin türü bir sayı olup olmadığını belirten bir Boolean değeri döndürür. |
| IS_OBJECT | Belirtilen ifade nin türünün JSON nesnesi olup olmadığını belirten bir Boolean değeri döndürür. |
| IS_PRIMITIVE | Belirtilen ifadenin türünün ilkel olup olmadığını belirten bir Boolean değeri döndürür (dize, Boolean, sayısal veya). `null` |
| IS_STRING | Belirtilen ifade nin türü bir dize olup olmadığını belirten bir Boolean değeri döndürür. |

Rota koşullarında aşağıdaki dize işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| CONCAT(x, y, ...) | İki veya daha fazla dize değeri birlikiyor sonucu bir dize döndürür. |
| UZUNLUK(x) | Belirtilen dize ifadesinin karakter sayısını döndürür.|
| ALT(x) | Büyük harf karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürür. |
| ÜST(x) | Küçük karakter verilerini büyük harfe dönüştürdükten sonra bir dize ifadesi döndürür. |
| SUBSTRING(string, start [, uzunluk]) | Dize ifadesinin belirtilen karakter sıfır tabanlı konumdan başlayan bir bölümünü döndürür ve belirtilen uzunluğa veya dize sonuna kadar devam ederse. |
| INDEX_OF(dize, parça) | İlk belirtilen dize ifadesi içinde ikinci dize ifadesinin ilk oluşumunun başlangıç konumunu döndürür veya dize bulunamazsa -1.|
| STARTS_WITH(x, y) | İlk dize ifadesinin ikinciyle başlayıp başlamadığını belirten bir Boolean döndürür. |
| ENDS_WITH(x, y) | İlk dize ifadesinin ikinciyle bitip bitmediğini belirten bir Boolean döndürür. |
| İçERİğİ(x,y) | İlk dize ifadesinin ikinciyi isayı içerip içerdirip içerdirmed |

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT SDK'larını](iot-hub-devguide-sdks.md)kullanarak uygulamalarınızda sorguları nasıl yürüterinizi öğrenin.