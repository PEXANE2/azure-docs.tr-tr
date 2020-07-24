---
title: Azure IoT Central telemetri, özellik ve komut yükleri | Microsoft Docs
description: Azure IoT Central cihaz şablonları, bir cihazın telemetri, Özellikler ve komutlarının uygulanması gerektiğini belirtmenize olanak tanır. Bir cihazın IoT Central ile değişimi için veri biçimini anlayın.
author: dominicbetts
ms.author: dobett
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 108a7940084e99348dc8fdfa0143d5c6855599df
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100312"
---
# <a name="telemetry-property-and-command-payloads"></a>Telemetri, özellik ve komut yükleri

_Bu makale, cihaz geliştiricileri için geçerlidir._

Azure IoT Central 'deki bir cihaz şablonu şunları tanımlayan bir şema ' dir:

* Bir cihazın IoT Central gönderdiği telemetri.
* Bir cihazın IoT Central ile eşitlendiği Özellikler.
* Bir cihazda IoT Central çağıran komutlar.

Bu makalede, cihaz geliştiricileri için cihazların bir cihaz şablonunda tanımlanan telemetri, Özellikler ve komutlar için gönderdikleri ve alabileceği JSON yükleri açıklanmaktadır.

Makale, olası telemetri, özellik ve komut yükünün her türünü tanımlamaz, ancak örnekler tüm anahtar türlerini gösterir.

Her örnek, cihazın IoT Central uygulamayla nasıl etkileşime gireceğini göstermek için türü ve örnek JSON yüklerini tanımlayan cihaz yetenek modeli 'nden (DCM) bir kod parçacığı gösterir.

DCM 'yi tanımlayan JSON dosyası [Digital Ikizi tanım dili (DTDL) v1](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/dtdlv1.md)kullanır. Bu belirtim `@id` özellik biçiminin tanımını içerir.

Kullanımda olan bu yüklerden bazılarını gösteren örnek cihaz kodu için bkz. [azure IoT Central uygulamanıza istemci uygulaması oluşturma ve bağlama (Node.js)](tutorial-connect-device-nodejs.md) ve [bir Istemci uygulamasını oluşturup Azure IoT Central uygulamanız (Python)](tutorial-connect-device-python.md) öğreticilerimize bağlama.

## <a name="view-raw-data"></a>Ham verileri görüntüleme

IoT Central, bir cihazın bir uygulamaya gönderdiği ham verileri görüntülemenize olanak sağlar. Bu görünüm, bir cihazdan gönderilen yükle ilgili sorunları gidermek için kullanışlıdır. Bir cihazın gönderdiği ham verileri görüntülemek için:

1. **Cihazlar** sayfasından cihaza gidin.

1. **Ham veri** sekmesini seçin:

    :::image type="content" source="media/concepts-telemetry-properties-commands/raw-data.png" alt-text="Ham veri görünümü":::
    
    Bu görünümde, görüntülenecek sütunları seçebilir ve görüntülenecek zaman aralığını ayarlayabilirsiniz. **Modellenmemiş veriler** sütunu, cihazdaki herhangi bir özellik veya telemetri tanımından hiçbiriyle eşleşmeyen verileri gösterir.


## <a name="telemetry"></a>Telemetri

### <a name="primitive-types"></a>İlkel türler

Bu bölümde, bir cihazın IoT Central uygulamasına akışı yapılan temel telemetri türleri örnekleri gösterilmektedir.

Bir DCM 'deki aşağıdaki kod parçacığında `boolean` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "BooleanTelemetry"
  },
  "name": "BooleanTelemetry",
  "schema": "boolean"
}
```

Bir cihaz istemcisi, aşağıdaki örnekte gösterildiği gibi Telemetriyi JSON olarak göndermelidir:

```json
{ "BooleanTelemetry": true }
```

Bir DCM 'deki aşağıdaki kod parçacığında `string` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "StringTelemetry"
  },
  "name": "StringTelemetry",
  "schema": "string"
}
```

Bir cihaz istemcisi, aşağıdaki örnekte gösterildiği gibi Telemetriyi JSON olarak göndermelidir:

```json
{ "StringTelemetry": "A string value - could be a URL" }
```

Bir DCM 'deki aşağıdaki kod parçacığında `integer` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "IntegerTelemetry"
  },
  "name": "IntegerTelemetry",
  "schema": "integer"
}

```

Bir cihaz istemcisi, aşağıdaki örnekte gösterildiği gibi Telemetriyi JSON olarak göndermelidir:

```json
{ "IntegerTelemetry": 23 }
```

Bir DCM 'deki aşağıdaki kod parçacığında `double` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DoubleTelemetry"
  },
  "name": "DoubleTelemetry",
  "schema": "double"
}
```

Bir cihaz istemcisi, aşağıdaki örnekte gösterildiği gibi Telemetriyi JSON olarak göndermelidir:

```json
{ "DoubleTelemetry": 56.78 }
```

Bir DCM 'deki aşağıdaki kod parçacığında `dateTime` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DateTimeTelemetry"
  },
  "name": "DateTimeTelemetry",
  "schema": "dateTime"
}
```

Bir cihaz istemcisi telemetrinin aşağıdaki örnek `DateTime` türler ıso 8061 uyumlu olmalıdır:

```json
{ "DateTimeTelemetry": "2020-08-30T19:16:13.853Z" }
```

Bir DCM 'deki aşağıdaki kod parçacığında `duration` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DurationTelemetry"
  },
  "name": "DurationTelemetry",
  "schema": "duration"
}
```

Bir cihaz istemcisi, aşağıdaki örnek süreler ISO 8601 süresi uyumlu olmalıdır:

```json
{ "DurationTelemetry": "PT10H24M6.169083011336625S" }
```

### <a name="complex-types"></a>Karmaşık türler

Bu bölümde, bir cihazın IoT Central uygulamasına akışını sağlayan karmaşık telemetri türleri örnekleri gösterilmektedir.

Bir DCM 'deki aşağıdaki kod parçacığında `geopoint` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "GeopointTelemetry"
  },
  "name": "GeopointTelemetry",
  "schema": "geopoint"
}
```

Bir cihaz istemcisi, aşağıdaki örneğe benzer şekilde Telemetriyi JSON olarak göndermelidir. IoT Central, değeri bir haritada PIN olarak görüntüler:

```json
{
  "GeopointTelemetry": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Bir DCM 'deki aşağıdaki kod parçacığında `Enum` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "EnumTelemetry"
  },
  "name": "EnumTelemetry",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Bir cihaz istemcisi, aşağıdaki örneğe benzer şekilde Telemetriyi JSON olarak göndermelidir. Olası değerler,, ve `0` `1` `2` IoT Central olarak, ve olarak görüntülenir `Item1` `Item2` `Item3` :

```json
{ "EnumTelemetry": 1 }
```

Bir DCM 'deki aşağıdaki kod parçacığında `Object` telemetri türünün tanımı gösterilmektedir. Bu nesne, ve türlerine sahip üç alana sahiptir `dateTime` `integer` `Enum` :

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "ObjectTelemetry"
  },
  "name": "ObjectTelemetry",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property1"
        },
        "name": "Property1",
        "schema": "dateTime"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property2"
        },
        "name": "Property2",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property3"
        },
        "name": "Property3",
        "schema": {
          "@id": "<element id>",
          "@type": "Enum",
          "displayName": {
            "en": "Enum"
          },
          "valueSchema": "integer",
          "enumValues": [
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item1"
              },
              "enumValue": 0,
              "name": "Item1"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item2"
              },
              "enumValue": 1,
              "name": "Item2"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item3"
              },
              "enumValue": 2,
              "name": "Item3"
            }
          ]
        }
      }
    ]
  }
}
```

Bir cihaz istemcisi, aşağıdaki örneğe benzer şekilde Telemetriyi JSON olarak göndermelidir. `DateTime`türler ISO 8061 uyumlu olmalıdır. İçin olası değerler,, ve `Property3` `0` `1` IoT Central olarak, ve olarak görüntülenir `Item1` `Item2` `Item3` :

```json
{
  "ObjectTelemetry": {
      "Property1": "2020-09-09T03:36:46.195Z",
      "Property2": 37,
      "Property3": 2
  }
}
```

Bir DCM 'deki aşağıdaki kod parçacığında `vector` telemetri türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "VectorTelemetry"
  },
  "name": "VectorTelemetry",
  "schema": "vector"
}
```

Bir cihaz istemcisi, aşağıdaki örnekte gösterildiği gibi Telemetriyi JSON olarak göndermelidir:

```json
{
  "VectorTelemetry": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="event-and-state-types"></a>Olay ve durum türleri

Bu bölümde, telemetri olaylarının örnekleri ve bir cihazın IoT Central uygulamasına gönderdiği durumlar gösterilmektedir.

Bir DCM 'deki aşağıdaki kod parçacığında bir olay türünün tanımı gösterilmektedir `integer` :

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/Event"
  ],
  "displayName": {
    "en": "IntegerEvent"
  },
  "name": "IntegerEvent",
  "schema": "integer"
}
```

Bir cihaz istemcisi, olay verilerini aşağıdaki örnekteki gibi görünen JSON olarak göndermelidir:

```json
{ "IntegerEvent": 74 }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir durum türünün tanımı gösterilmektedir `integer` :

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/State"
  ],
  "displayName": {
    "en": "IntegerState"
  },
  "name": "IntegerState",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level1"
        },
        "enumValue": 1,
        "name": "Level1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level2"
        },
        "enumValue": 2,
        "name": "Level2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level3"
        },
        "enumValue": 3,
        "name": "Level3"
      }
    ]
  }
}
```

Bir cihaz istemcisi, durumu aşağıdaki örnekte gösterildiği gibi JSON olarak göndermelidir. Olası tamsayı durum değerleri,, veya ' dir `1` `2` `3` :

```json
{ "IntegerState": 2 }
```

## <a name="properties"></a>Özellikler

> [!NOTE]
> Özellikler için yük biçimleri 07/14/2020 tarihinde veya sonrasında oluşturulan uygulamalar için geçerlidir.

### <a name="primitive-types"></a>İlkel türler

Bu bölümde, bir cihazın IoT Central uygulamasına gönderdiği temel özellik türlerinin örnekleri gösterilmektedir.

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `boolean` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "BooleanProperty"
  },
  "name": "BooleanProperty",
  "schema": "boolean"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{ "BooleanProperty": false }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `boolean` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "LongProperty"
  },
  "name": "LongProperty",
  "schema": "long"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{ "LongProperty": 439 }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `date` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DateProperty"
  },
  "name": "DateProperty",
  "schema": "date"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir. `Date`türler ISO 8061 uyumlu olmalıdır:

```json
{ "DateProperty": "2020-05-17" }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `duration` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DurationProperty"
  },
  "name": "DurationProperty",
  "schema": "duration"
}
```

Bir cihaz istemcisi, cihazda bildirilen bir özellik olarak aşağıdaki örnekte gösterildiği gibi bir JSON yükü göndermelidir ikizi-süreler ISO 8601 süresi uyumlu olmalıdır:

```json
{ "DurationProperty": "PT10H24M6.169083011336625S" }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `float` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "FloatProperty"
  },
  "name": "FloatProperty",
  "schema": "float"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{ "FloatProperty": 1.9 }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `string` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringProperty"
  },
  "name": "StringProperty",
  "schema": "string"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{ "StringProperty": "A string value - could be a URL" }
```

### <a name="complex-types"></a>Karmaşık türler

Bu bölümde, bir cihazın IoT Central uygulamasına gönderdiği karmaşık özellik türleri örnekleri gösterilmektedir.

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `geopoint` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "GeopointProperty"
  },
  "name": "GeopointProperty",
  "schema": "geopoint"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{
  "GeopointProperty": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `Enum` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumProperty"
  },
  "name": "EnumProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir. Olası değerler,, ve `0` `1` IoT Central olarak, ve olarak görüntülenir `Item1` `Item2` `Item3` :

```json
{ "EnumProperty": 1 }
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `Object` . Bu nesne, türlerine sahip iki alana sahiptir `string` ve `integer` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "ObjectProperty"
  },
  "name": "ObjectProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field1"
        },
        "name": "Field1",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field2"
        },
        "name": "Field2",
        "schema": "string"
      }
    ]
  }
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{
  "ObjectProperty": {
    "Field1": 37,
    "Field2": "A string value"
  }
}
```

Bir DCM 'deki aşağıdaki kod parçacığında bir özellik türünün tanımı gösterilmektedir `vector` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "VectorProperty"
  },
  "name": "VectorProperty",
  "schema": "vector"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

```json
{
  "VectorProperty": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="writeable-property-types"></a>Yazılabilir Özellik türleri

Bu bölümde, bir cihazın IoT Central uygulamasından aldığı yazılabilir Özellik türlerinin örnekleri gösterilmektedir.

IoT Central cihazdan yazılabilir Özellik güncelleştirmelerine yanıt bekliyor. Yanıt iletisi `ac` ve `av` alanlarını içermelidir. `ad` alanı isteğe bağlıdır. Örnekler için aşağıdaki kod parçacıklarına bakın.

`ac`, aşağıdaki tablodaki değerleri kullanan sayısal bir alandır:

| Değer | Etiketle | Açıklama |
| ----- | ----- | ----------- |
| `'ac': 200` | Tamamlandı | Özellik değiştirme işlemi başarıyla tamamlandı. |
| `'ac': 202`veya`'ac': 201` | Beklemede | Özellik değiştirme işlemi bekliyor veya devam ediyor |
| `'ac': 4xx` | Hata | İstenen özellik değişikliği geçerli değil veya bir hata oluştu |
| `'ac': 5xx` | Hata | Cihaz, istenen değişikliği işlerken beklenmeyen bir hatayla karşılaştı. |

`av`cihaza gönderilen sürüm numarasıdır.

`ad`, bir seçenek dize açıklamasıdır.

Bir DCM 'deki aşağıdaki kod parçacığında yazılabilir `string` özellik türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringPropertyWritable"
  },
  "name": "StringPropertyWritable",
  "writable": true,
  "schema": "string"
}
```

Cihaz IoT Central aşağıdaki yükü alır:

```json
{  
  "StringPropertyWritable": "A string from IoT Central", "$version": 7
}
```

Cihaz, güncelleştirmeyi tamamladıktan sonra IoT Central aşağıdaki JSON yükünü göndermelidir. Bu ileti, IoT Central alınan özgün güncelleştirmenin sürüm numarasını içerir. IoT Central bu iletiyi aldığında, özelliği Kullanıcı arabiriminde **eşitlenmiş** olarak işaretler:

```json
{
  "StringPropertyWritable": {
    "value": "A string from IoT Central",
    "ac": 200,
    "ad": "completed",
    "av": 7
  }
}
```

Bir DCM 'deki aşağıdaki kod parçacığında yazılabilir `Enum` özellik türünün tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumPropertyWritable"
  },
  "name": "EnumPropertyWritable",
  "writable": true,
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Cihaz IoT Central aşağıdaki yükü alır:

```json
{  
  "EnumPropertyWritable":  1 , "$version": 10
}
```

Cihaz, güncelleştirmeyi tamamladıktan sonra IoT Central aşağıdaki JSON yükünü göndermelidir. Bu ileti, IoT Central alınan özgün güncelleştirmenin sürüm numarasını içerir. IoT Central bu iletiyi aldığında, özelliği Kullanıcı arabiriminde **eşitlenmiş** olarak işaretler:

```json
{
  "EnumPropertyWritable": {
    "value": 1,
    "ac": 200,
    "ad": "completed",
    "av": 10
  }
}
```

## <a name="commands"></a>Komutlar

### <a name="synchronous-command-types"></a>Zaman uyumlu komut türleri

Bir DCM 'nin aşağıdaki kod parçacığında parametresi olmayan ve cihazın herhangi bir şeyi döndürmesini beklemediği zaman uyumlu bir komutun tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "displayName": {
    "en": "SynchronousCommandBasic"
  },
  "name": "SynchronousCommandBasic"
}
```

Cihaz, istekte boş bir yük alır ve `200` başarıyı göstermek için http yanıt kodu ile yanıtta boş bir yük döndürmelidir.

Bir DCM 'nin aşağıdaki kod parçacığında, bir tamsayı parametresi olan ve cihazın bir tamsayı değeri döndürmesini bekleyen zaman uyumlu bir komutun tanımı gösterilmektedir:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "SynchronousCommandSimple"
  },
  "name": "SynchronousCommandSimple"
}
```

Cihaz, istek yükü olarak bir tamsayı değeri alır. Bu cihaz, `200` başarıyı göstermek için bir http yanıt kodu ile yanıt yükü olarak bir tamsayı değeri döndürmelidir.

Bir DCM 'deki aşağıdaki kod parçacığında, bir nesne parametresi olan ve cihazın bir nesne döndürmesini bekleyen zaman uyumlu bir komutun tanımı gösterilmektedir. Bu örnekte, her iki nesnenin de Integer ve String alanları vardır:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": {
      "@id": "<element id>",
      "@type": "Object",
      "displayName": {
        "en": "Object"
      },
      "fields": [
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field1"
          },
          "name": "Field1",
          "schema": "integer"
        },
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field2"
          },
          "name": "Field2",
          "schema": "string"
        }
      ]
    }
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": {
      "@id": "<element id>",
      "@type": "Object",
      "displayName": {
        "en": "Object"
      },
      "fields": [
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field1"
          },
          "name": "Field1",
          "schema": "integer"
        },
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field2"
          },
          "name": "Field2",
          "schema": "string"
        }
      ]
    }
  },
  "displayName": {
    "en": "SynchronousCommandComplex"
  },
  "name": "SynchronousCommandComplex"
}
```

Aşağıdaki kod parçacığında cihaza gönderilen örnek bir istek yükü gösterilmektedir:

```json
{ "Field1": 56, "Field2": "A string value" }
```

Aşağıdaki kod parçacığında cihazdan gönderilen örnek bir yanıt yükü gösterilmektedir. `200`Başarıyı göstermek için bir http yanıt kodu kullanın:

```json
{ "Field1": 87, "Field2": "Another string value" }
```

### <a name="asynchronous-command-types"></a>Zaman uyumsuz komut türleri

Bir DCM 'deki aşağıdaki kod parçacığında bir zaman uyumsuz komutun tanımı gösterilmektedir. Komutun bir tamsayı parametresi vardır ve cihazın bir tamsayı değeri döndürmesini bekler:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "asynchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "AsynchronousCommandSimple"
  },
  "name": "AsynchronousCommandSimple"
}
```

Cihaz, istek yükü olarak bir tamsayı değeri alır. Cihazın `202` zaman uyumsuz işleme isteğini kabul ettiğini belirten bir http yanıt kodu ile birlikte boş bir yanıt yükü döndürmelidir.

Cihaz isteği işlemeyi tamamladığında, aşağıdaki örnek gibi görünen IoT Central bir özellik göndermelidir. Özellik adı, komut adıyla aynı olmalıdır:

```json
{
  "AsynchronousCommandSimple": {
    "value": 87
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz geliştiricisi olarak, artık "cihaz şablonlarının öğrenilmesi" sayesinde, cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarının IoT Central nasıl güvenlik altına aldığı hakkında daha fazla bilgi edinmek için [Azure IoT Central 'a bağlanma](./concepts-get-connected.md) hakkında daha fazla bilgi almak üzere önerilen bir sonraki adım.
