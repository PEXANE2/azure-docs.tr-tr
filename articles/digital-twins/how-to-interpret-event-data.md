---
title: Olay verilerini yorumlama
titleSuffix: Azure Digital Twins
description: Farklı olay türlerini ve bunların farklı bildirim iletilerini yorumlama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: d32ad29bf652cad62a5950859ebff0366e09fc6f
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88510037"
---
# <a name="understand-event-data"></a>Olay verilerini anlama

Azure Digital TWINS 'deki farklı olaylar, farklı eylemler meydana geldiğinde çözüm arka ucunun farkında olmasını sağlayan **Bildirimler**üretir. Bunlar daha sonra bu bilgileri işlem yapmak için kullanabileceğiniz Azure dijital TWINS içindeki ve dışındaki farklı konumlara [yönlendirilir](concepts-route-events.md) .

Oluşturulabilecek birçok bildirim türü vardır ve bildirim iletileri, hangi olay türüne bağlı olarak farklı görünebilir. Bu makale, farklı ileti türleri ve nasıl görünebilecekleri hakkında ayrıntılı bilgi sağlar.

Bu grafik farklı bildirim türlerini gösterir:

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

Genel olarak, bildirimler iki bölümden oluşur: başlık ve gövde. 

### <a name="event-notification-headers"></a>Olay bildirim üstbilgileri

Bildirim iletisi üstbilgileri, anahtar-değer çiftleri ile temsil edilir. Kullanılan protokole (MQTT, AMQP veya HTTP) bağlı olarak, ileti üstbilgileri farklı şekilde serileştirilir. Bu bölümde, seçilen protokole ve Serileştirmeden bağımsız olarak bildirim iletilerine ilişkin genel üst bilgi bilgileri anlatılmaktadır.

Bazı bildirimler CloudEvents standardına uygundur. CloudEvents uygunluğu aşağıdaki gibidir.
* Cihazlardan alınan bildirimler, bildirim için mevcut belirtimleri izlemeye devam eder
* IoT Hub tarafından işlenen ve yayılan bildirimler, IoT Hub, Event Grid gibi CloudEvents desteğini desteklemeyi tercih ettiği durumlar haricinde bildirim için mevcut belirtimleri izlemeye devam eder.
* Bir [model](concepts-models.md) ile [dijital TWINS](concepts-twins-graph.md) 'Ten alınan bildirimler, cloudevents ile uyumlu
* Azure dijital TWINS tarafından işlenen ve yayılan bildirimler, CloudEvents ile uyumlu

Hizmetlerin, sıralarını belirtmek için tüm bildirimlere bir sıra numarası eklemesi veya kendi sıralamasını başka bir şekilde korumanız gerekir. 

Azure dijital TWINS tarafından Event Grid 'e yapılan bildirimler, olay Kılavuzu konusunda tanımlanan şema türüne bağlı olarak, CloudEvents şeması veya EventGridEvent şemasına otomatik olarak biçimlendirilir. 

Üst bilgilerdeki uzantı öznitelikleri, yük içindeki Event Grid şemasına özellikler olarak eklenecektir. 

### <a name="event-notification-bodies"></a>Olay bildirim gövdeleri

Bildirim iletilerinin gövdeleri burada JSON 'da açıklanmıştır. İleti gövdesi (örneğin, JSON, CBOR, Protoda vb.) için istenen seri hale getirme öğesine bağlı olarak, ileti gövdesi farklı şekilde seri hale getirilebilir.

Gövdenin içerdiği alan kümesi, farklı bildirim türleriyle farklılık gösterir. Bu iki örnek ileti gövdeinin, genel olarak neye benzedikleri ve dahil olabileceği hakkında fikir sahibi olmak için.

Telemetri iletisi:

```json
{
  "specversion": "1.0",
  "id": "df5a5992-817b-4e8a-b12c-e0b18d4bf8fb",
  "type": "microsoft.iot.telemetry",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net/digitaltwins/room1",
  "data": {
    "Temperature": 10
  },
  "dataschema": "dtmi:example:com:floor4;2",
  "datacontenttype": "application/json",
  "traceparent": "00-7e3081c6d3edfb4eaf7d3244b2036baa-23d762f4d9f81741-01"
}
```

Yaşam döngüsü bildirim iletisi:

```json
{
  "specversion": "1.0",
  "id": "d047e992-dddc-4a5a-b0af-fa79832235f8",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net",
  "data": {
    "$dtId": "floor1",
    "$etag": "W/\"e398dbf4-8214-4483-9d52-880b61e491ec\"",
    "$metadata": {
      "$model": "dtmi:example:Floor;1"
    }
  },
  "subject": "floor1",
  "time": "2020-06-23T19:03:48.9700792Z",
  "datacontenttype": "application/json",
  "traceparent": "00-18f4e34b3e4a784aadf5913917537e7d-691a71e0a220d642-01"
}
```

## <a name="message-format-detail-for-different-event-types"></a>Farklı olay türleri için ileti biçimi ayrıntısı

Bu bölüm IoT Hub ve Azure dijital TWINS (veya diğer Azure IoT Hizmetleri) tarafından yayınlanan farklı bildirim türleri hakkında daha fazla ayrıntıya gider. Her bildirim türünü tetikleyen şeyleri ve her bir bildirim gövdesi türüyle birlikte bulunan alan kümesini okuyacaksınız.

### <a name="digital-twin-life-cycle-notifications"></a>Dijital ikizi yaşam döngüsü bildirimleri

Tüm [dijital TWINS](concepts-twins-graph.md) , [Azure dijital twıns 'de IoT Hub cihazları](how-to-ingest-iot-hub-data.md) temsil etmeksizin bağımsız olarak bildirimleri yayar. Bunun nedeni, dijital ikizi kendisi ile ilgili olan **yaşam döngüsü bildirimlerinin**bir kendisidir.

Yaşam döngüsü bildirimleri şu durumlarda tetiklenir:
* Dijital bir ikizi oluşturulur
* Dijital ikizi silindi

#### <a name="properties"></a>Özellikler

İşte bir yaşam döngüsü bildiriminin gövdesinde yer alan alanlar.

| Name | Değer |
| --- | --- |
| `id` | Bir UUID veya hizmet tarafından tutulan bir sayaç gibi bildirimin tanımlayıcısı. `source` + `id` her farklı olay için benzersizdir. |
| `source` | *Myhub.Azure-Devices.net* veya *mydigitaltwins.westus2.azuredigitaltwins.net* gibi IoT Hub veya Azure dijital TWINS örneğinin adı |
| `specversion` | *1,0*<br>İleti, CloudEvents belirtiminin bu sürümüne uyar. |
| `type` | `Microsoft.DigitalTwins.Twin.Create`<br>`Microsoft.DigitalTwins.Twin.Delete` |
| `datacontenttype` | `application/json` |
| `subject` | Dijital ikizi KIMLIĞI |
| `time` | İkizi üzerinde işlem gerçekleştiği zaman damgası |
| `traceparent` | Etkinlik için bir W3C Trace bağlamı |

#### <a name="body-details"></a>Gövde ayrıntıları

Gövde, JSON biçiminde gösterilen etkilenen dijital ikizi. Bu, *dijital TWINS kaynağı 7,1*' dir.

Oluşturma olayları için yük, kaynak oluşturulduktan sonra ikizi durumunu yansıtır, bu nedenle tüm sistem tarafından oluşturulan öğeleri bir çağrıda olduğu gibi içermelidir `GET` .

Aşağıda, bir [ıot Tak ve kullan (PnP)](../iot-pnp/overview-iot-plug-and-play.md) cihazının, bileşenler ve en üst düzey özellikleri olmayan bir gövde örneği verilmiştir. Cihazlar için anlamlı olmayan Özellikler (bildirilen özellikler gibi) atlanmalıdır.

```json
{
  "$dtId": "device-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "thermostat": {
    "temperature": 80,
    "humidity": 45,
    "$metadata": {
      "$model": "dtmi:com:contoso:Thermostat;1",
      "temperature": {
        "desiredValue": 85,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      },
      "humidity": {
        "desiredValue": 40,
        "desiredVersion": 1,
        "ackVersion": 1,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Thermostat_X500;1",
  }
}
```

Dijital ikizi başka bir örneği aşağıda verilmiştir. Bu, bir [modeli](concepts-models.md)temel alır ve bileşenleri desteklemez:

```json
{
  "$dtId": "logical-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "avgTemperature": 70,
  "comfortIndex": 85,
  "$metadata": {
    "$model": "dtmi:com:contoso:Building;1",
    "avgTemperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "comfortIndex": {
      "desiredValue": 90,
      "desiredVersion": 1,
      "ackVersion": 3,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

### <a name="digital-twin-relationship-change-notifications"></a>Digital ikizi ilişki değişikliği bildirimleri

**İlişki değişikliği bildirimleri** , dijital ikizi herhangi bir ilişkisi oluşturulduğunda, güncelleştirilirken veya silindiğinde tetiklenir. 

#### <a name="properties"></a>Özellikler

Bir Edge değişiklik bildiriminin gövdesinde yer alan alanlar aşağıda verilmiştir.

| Name    | Değer |
| --- | --- |
| `id` | Bir UUID veya hizmet tarafından tutulan bir sayaç gibi bildirimin tanımlayıcısı. `source` + `id` her farklı olay için benzersizdir |
| `source` | *Mydigitaltwins.westus2.azuredigitaltwins.net* gibi Azure dijital TWINS örneğinin adı |
| `specversion` | *1,0*<br>İleti, CloudEvents belirtiminin bu sürümüne uyar. |
| `type` | `Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br>`Microsoft.DigitalTwins.Relationship.Delete`
|`datacontenttype`| `application/json` |
| `subject` | İlişkinin KIMLIĞI, örneğin `<twinID>/relationships/<relationshipID>` |
| `time` | İlişki üzerinde işlem gerçekleştiği zaman damgası |
| `traceparent` | Etkinlik için bir W3C Trace bağlamı |

#### <a name="body-details"></a>Gövde ayrıntıları

Gövde, JSON biçiminde bir ilişkinin yüküyle aynı olur. Bu, `GET` [Digitaltwins API 'si](how-to-use-apis-sdks.md)aracılığıyla bir ilişki isteğiyle aynı biçimi kullanır. 

"İlişki güncelleştirme", ilişkinin özelliklerinin değiştiği anlamına gelir. 

Bir özelliği güncelleştirmek için bir güncelleştirme ilişkisi bildirimine bir örnek aşağıda verilmiştir:

```json
{
    "modelId": "dtmi:example:Floor;1",
    "patch": [
      {
        "value": "user3",
        "path": "/ownershipUser",
        "op": "replace"
      }
    ]
  }
```

İçin `Relationship.Delete` , gövde `GET` istekle aynıdır ve silinmeden önce en son durumu alır.

İlişki oluşturma veya silme bildirimine bir örnek aşağıda verilmiştir:

```json
{
    "$relationshipId": "device_to_device",
    "$etag": "W/\"72479873-0083-41a8-83e2-caedb932d881\"",
    "$relationshipName": "Connected",
    "$targetId": "device2",
    "connectionType": "WIFI"
}
```

### <a name="digital-twin-change-notifications"></a>Dijital ikizi değişiklik bildirimleri

Dijital **ikizi değişiklik bildirimleri** , bir dijital ikizi güncelleştirilirken tetiklenir, örneğin:
* Özellik değerleri veya meta veriler değiştiğinde.
* Dijital ikizi veya bileşen meta verileri değiştiğinde. Bu senaryoya bir örnek, dijital bir ikizi modelini değiştiriyor.

#### <a name="properties"></a>Özellikler

Dijital ikizi değişiklik bildiriminin gövdesinde yer alan alanlar aşağıda verilmiştir.

| Name    | Değer |
| --- | --- |
| `id` | Bir UUID veya hizmet tarafından tutulan bir sayaç gibi bildirimin tanımlayıcısı. `source` + `id` her farklı olay için benzersizdir |
| `source` | *Myhub.Azure-Devices.net* veya *mydigitaltwins.westus2.azuredigitaltwins.net* gibi IoT Hub veya Azure dijital TWINS örneğinin adı
| `specversion` | *1,0*<br>İleti, CloudEvents belirtiminin bu sürümüne uyar. |
| `type` | `Microsoft.DigitalTwins.Twin.Update` |
| `datacontenttype` | `application/json` |
| `subject` | Dijital ikizi KIMLIĞI |
| `time` | İşlem dijital ikizi üzerinde gerçekleştiyse zaman damgası |
| `traceparent` | Etkinlik için bir W3C Trace bağlamı |

#### <a name="body-details"></a>Gövde ayrıntıları

Bildirimin gövdesi, `Twin.Update` dijital ikizi güncelleştirmesini içeren BIR JSON yama belgesidir.

Örneğin, bir dijital ikizi, aşağıdaki düzeltme eki kullanılarak güncelleştirildiğini varsayalım.

```json
[
    {
        "op": "replace",
        "value": 40,
        "path": "/Temperature"
    },
    {
        "op": "add",
        "value": 30,
        "path": "/comp1/prop1"
    }
]
```

Karşılık gelen bildirim (Azure Digital TWINS bir dijital ikizi güncelleştiren gibi hizmet tarafından zaman uyumlu olarak yürütülürse) şöyle bir gövdeye sahip olur:

```json
{
    "modelId": "dtmi:example:com:floor4;2",
    "patch": [
      {
        "value": 40,
        "path": "/Temperature",
        "op": "replace"
      },
      {
        "value": 30,
        "path": "/comp1/prop1",
        "op": "add"
      }
    ]
  }
```

## <a name="next-steps"></a>Sonraki adımlar

Olayları iletmek için uç noktalar ve rotalar oluşturma konusuna bakın:
* [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes-apis-cli.md)

Ya da Azure Digital TWINS API 'Leri ve SDK seçenekleri hakkında daha fazla bilgi edinin:
* [*Nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md)