---
title: Kullanıcı tanımlı işlevler istemci kitaplığı başvurusu - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins kullanıcı tanımlı işlevler istemci kitaplığı başvuru belgeleri.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: bd6095daca51ddca0cfb4b34ca86e763df9a3d02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276807"
---
# <a name="user-defined-functions-client-library-reference"></a>Kullanıcı tanımlı işlevler istemci kitaplığı başvurusu

Bu belge, Azure Digital Twins kullanıcı tanımlı işlevler istemci kitaplığı için başvuru bilgileri sağlar.

## <a name="helper-methods"></a>Yardımcı yöntemler

İstemci kitaplığı, sık kullanılan işlemler için yardımcı yöntemleri tanımlar.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) 』`space`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev grafikten alanı alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *Kimliği*  | `guid` | Boşluk tanımlayıcısı |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) 』`sensor`

Sensör tanımlayıcısı verildiğinde, bu işlev sensörü grafikten alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *Kimliği*  | `guid` | Sensör tanımlayıcısı |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) 』`device`

Bir aygıt tanımlayıcısı verildiğinde, bu işlev aygıtı grafikten alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Kimliği* | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) 』`value`

Bir sensör tanımlayıcısı ve veri türü göz önüne alındığında, bu işlev sensörün geçerli değerini alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Sensör tanımlayıcısı |
| *Datatype*  | `string` | Sensör veri türü |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) 』`value`

Bir boşluk tanımlayıcısı ve değer adı verilen bu işlev, o uzay özelliği için geçerli değeri alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Boşluk tanımlayıcısı |
| *valueName* | `string` | Alan özelliği adı |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) 』`value[]`

Bir sensör tanımlayıcısı ve veri türü göz önüne alındığında, bu işlev sensörün geçmiş değerlerini alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensör tanımlayıcısı |
| *Datatype* | `string` | Sensör veri türü |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) 』`value[]`

Bir boşluk tanımlayıcısı ve değer adı verilen bu işlev, alandaki bu özelliğin geçmiş değerlerini alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |
| *valueName* | `string` | Alan özelliği adı |

### <a name="getspacechildspacesspaceid--space"></a>almakSpaceChildSpaces(spaceId) 』`space[]`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev bu üst alan için alt alanları alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) 』`sensor[]`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev o üst alan için alt sensörleri alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) 』`device[]`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev o üst alan için alt aygıtları alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) 』`sensor[]`

Bir aygıt tanımlayıcısı verildiğinde, bu işlev bu üst aygıtın alt sensörlerini alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Cihaz tanımlayıcısı |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) 』`space`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev üst alanını alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *çocukSpaceId* | `guid` | Boşluk tanımlayıcısı |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) 』`space`

Sensör tanımlayıcısı verildiğinde, bu işlev üst alanını alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensör tanımlayıcısı |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) 』`space`

Bir aygıt tanımlayıcısı verildiğinde, bu işlev üst alanını alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) 』`space`

Bir sensör tanımlayıcısı verildiğinde, bu işlev ana aygıtını alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensör tanımlayıcısı |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) 』`extendedProperty`

Bir boşluk tanımlayıcısı verildiğinde, bu işlev özelliği ve değerini boşluktan alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |
| *Propertyname* | `string` | Alan özelliği adı |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) 』`extendedProperty`

Bir sensör tanımlayıcısı verildiğinde, bu işlev özelliği ve değerini sensörden alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensör tanımlayıcısı |
| *Propertyname* | `string` | Sensör özellik adı |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) 』`extendedProperty`

Bir aygıt tanımlayıcısı verildiğinde, bu işlev özelliği ve değerini aygıttan alır.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Cihaz tanımlayıcısı |
| *Propertyname* | `string` | Aygıt özelliği adı |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, değer)

Bu işlev, verilen veri türüyle sensör nesnesine bir değer ayarlar.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensör tanımlayıcısı |
| *Datatype*  | `string` | Sensör veri türü |
| *value*  | `string` | Değer |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, değer)

Bu işlev, verilen veri türüyle alan nesnesi üzerinde bir değer ayarlar.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Boşluk tanımlayıcısı |
| *Datatype* | `string` | Veri türü |
| *value* | `string` | Değer |

### <a name="logmessage"></a>günlük(mesaj)

Bu işlev, kullanıcı tanımlı işlev içinde aşağıdaki iletiyi günlüğe kaydeder.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *İleti* | `string` | Günlüğe kaydedilecek ileti |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topolojiObjectId, topolojiObjectType, yük)

Bu işlev, gönderilmek üzere özel bir bildirim gönderir.

**Tür**: global fonksiyon

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *topolojiObjectId*  | `guid` | Grafik nesne tanımlayıcısı. Örnekler uzay, sensör ve aygıt kimliğidir.|
| *topolojiObjectType*  | `string` | Örnekler sensör ve cihazdır.|
| *yük*  | `string` | JSON yükü bildirimle birlikte gönderilecek. |

## <a name="return-types"></a>İade türleri

İstemci başvuru yardımcısı yöntemlerinden döndürülen yanıt modelleri aşağıda açıklanmıştır.

### <a name="space"></a>Alan

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
}
```

### <a name="space-methods"></a>Uzay yöntemleri

#### <a name="parent--space"></a>Veli()`space`

Bu işlev, geçerli alanın üst alanını döndürür.

#### <a name="childsensors--sensor"></a>Çocuk Sensörleri()`sensor[]`

Bu fonksiyon, geçerli alanın alt sensörlerini döndürür.

#### <a name="childdevices--device"></a>ChildDevices()`device[]`

Bu işlev, geçerli alanın alt aygıtlarını döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) 』`extendedProperty`

Bu işlev, uzatılmış özelliği ve geçerli alan için değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Propertyname* | `string` | Genişletilmiş özelliğin adı |

#### <a name="valuevaluename--value"></a>Değer(valueName) 』`value`

Bu işlev, geçerli alanın değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Değerin adı |

#### <a name="historyvaluename--value"></a>Tarihçe(valueName) 』`value[]`

Bu işlev, geçerli alanın geçmiş değerlerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Değerin adı |

#### <a name="notifypayload"></a>Bildirme (yük)

Bu işlev, belirtilen yükü içeren bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *yük* | `string` | JSON yükü bildirime dahil etmek |

### <a name="device"></a>Cihaz

```JSON
{
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Cihaz yöntemleri

#### <a name="parent--space"></a>Veli()`space`

Bu işlev, geçerli aygıtın üst alanını döndürür.

#### <a name="childsensors--sensor"></a>Çocuk Sensörleri()`sensor[]`

Bu işlev, geçerli cihazın alt sensörlerini döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) 』`extendedProperty`

Bu işlev, uzatılmış özelliği ve geçerli aygıt için değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Propertyname* | `string` | Genişletilmiş özelliğin adı |

#### <a name="notifypayload"></a>Bildirme (yük)

Bu işlev, belirtilen yükü içeren bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *yük* | `string` | JSON yükü bildirime dahil etmek |

### <a name="sensor"></a>Sensör

```JSON
{
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
}
```

### <a name="sensor-methods"></a>Sensör yöntemleri

#### <a name="space--space"></a>Boşluk()`space`

Bu işlev, geçerli sensörün ana alanını döndürür.

#### <a name="device--device"></a>Cihaz()`device`

Bu işlev, geçerli sensörün ana aygıtını döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) 』`extendedProperty`

Bu işlev, uzatılmış özelliği ve geçerli sensör için değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Propertyname* | `string` | Genişletilmiş özelliğin adı |

#### <a name="value--value"></a>Değer()`value`

Bu fonksiyon geçerli sensörün değerini döndürür.

#### <a name="history--value"></a>Tarihçe()`value[]`

Bu işlev, geçerli sensörün geçmiş değerlerini döndürür.

#### <a name="notifypayload"></a>Bildirme (yük)

Bu işlev, belirtilen yükü içeren bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *yük* | `string` | JSON yükü bildirime dahil etmek |

### <a name="value"></a>Değer

```JSON
{
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Genişletilmiş özellik

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Digital Twins kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md)hakkında bilgi edinin.

- [Kullanıcı tanımlı işlevleri nasıl oluşturabilirsiniz](./how-to-user-defined-functions.md)öğrenin.

- [Kullanıcı tanımlı işlevleri nasıl hata ayıklama yı](./how-to-diagnose-user-defined-functions.md)öğreneceksin.
