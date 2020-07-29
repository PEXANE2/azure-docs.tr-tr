---
title: IoT Tak ve Kullan dijital TWINS 'i anlama
description: IoT Tak ve Kullan önizlemesi 'nin dijital TWINS 'i nasıl kullandığını anlayın
author: prashmo
ms.author: prashmo
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1908abfb3d0ea20c69a68344d54076c6760e9e63
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352418"
---
# <a name="understand-iot-plug-and-play-digital-twins"></a>IoT Tak ve Kullan dijital TWINS 'i anlama

IoT Tak ve Kullan cihazı, [dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) şeması tarafından tanımlanan bir modeli uygular. Model, belirli bir cihazın sahip olduğu bileşenler, özellikler, komutlar ve telemetri iletileri kümesini açıklar. Bir IoT Hub 'ına ilk kez bağlantı Tak ve Kullan bir cihaz ikizi ve bir Digital ikizi başlatılır.

IoT Tak ve Kullan DTDL sürüm 2 kullanır. Bu sürüm hakkında daha fazla bilgi için bkz. GitHub 'da [dijital TWINS tanım dili (DTDL)-sürüm 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) belirtimi.

DTDL, IoT Tak ve Kullan özel değildir. [Azure dijital TWINS](../digital-twins/overview.md)gibi diğer IoT Hizmetleri, binalar ve enerji ağları gibi tüm ortamları temsil etmek için bunu kullanır. Daha fazla bilgi için bkz. [Azure dijital TWINS 'de ikizi modellerini anlama](../digital-twins/concepts-models.md).

Bu makalede, bileşenlerin ve özelliklerin bir cihaz ikizi *istenen* ve *bildirilen* bölümlerinde nasıl temsil edildiği açıklanmaktadır. Ayrıca, bu kavramların ilgili dijital ikizi nasıl eşlendiğini de açıklar.

Bu makaledeki IoT bağlama ve yürütme aygıtı, [termostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) bileşeniyle [sıcaklık denetleyicisi modelini](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) uygular.

## <a name="device-twins-and-digital-twins"></a>Cihaz ikları ve dijital TWINS

Cihaz TWINS, meta veriler, konfigürasyonlar ve koşullar dahil olmak üzere cihaz durum bilgilerini depolayan JSON belgelerdir. Daha fazla bilgi edinmek için bkz. [IoT Hub 'da cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md). Hem cihaz hem de çözüm oluşturucular, IoT Tak ve Kullan kurallarını kullanarak cihaz ve çözümleri uygulamak için aynı cihaz Ikizi API 'Leri ve SDK 'ları kullanmaya devam edebilir.

Dijital Ikizi API 'Leri, bileşenler, Özellikler ve komutlar gibi dijital TWINS tanım dili 'nde (DTDL) üst düzey yapılar üzerinde çalışır. Dijital Ikizi API 'Leri, çözüm oluşturucuların IoT Tak ve Kullan çözümleri oluşturmalarına daha kolay hale getirir.

Bir cihaz ikizi, yazılabilir bir özelliğin durumu istenen ve bildirilen bölümler arasında bölünür. Tüm salt okunurdur özellikler, bildirilen bölümünde bulunur.

Dijital bir ikizi, özelliğin geçerli ve istenen durumunun birleştirilmiş bir görünümü vardır. Belirli bir özelliğin eşitleme durumu karşılık gelen kök düzeyi veya bileşen `$metadata` bölümünde saklanır.

### <a name="digital-twin-json-format"></a>Digital ikizi JSON biçimi

JSON nesnesi olarak temsil edildiğinde, dijital bir ikizi aşağıdaki alanları içerir:

| Alan adı | Açıklama |
| --- | --- |
| `$dtId` | Cihazın dijital ikizi KIMLIĞINI temsil eden kullanıcı tarafından sağlanmış dize |
| `{propertyName}` | JSON içindeki bir özelliğin değeri |
| `$metadata.$model` | Seçim Bu dijital ikizi karakterleştirir model arabiriminin KIMLIĞI |
| `$metadata.{propertyName}.desiredValue` | [Yalnızca yazılabilir özellikler için] Belirtilen özelliğin istenen değeri |
| `$metadata.{propertyName}.desiredVersion` | [Yalnızca yazılabilir özellikler için] IoT Hub tarafından tutulan istenen değerin sürümü|
| `$metadata.{propertyName}.ackVersion` | [Gerekli, yalnızca yazılabilir özellikler için] Dijital ikizi uygulayan cihaz tarafından kabul edilen sürüm, istenen sürüme eşit veya ondan daha büyük olmalıdır |
| `$metadata.{propertyName}.ackCode` | [Gerekli, yalnızca yazılabilir özellikler için] `ack`Dijital ikizi uygulayan cihaz uygulaması tarafından döndürülen kod |
| `$metadata.{propertyName}.ackDescription` | [İsteğe bağlı, yalnızca yazılabilir özellikler için] `ack`Dijital ikizi uygulayan cihaz uygulaması tarafından döndürülen açıklama |
| `$metadata.{propertyName}.lastUpdateTime` | IoT Hub, özelliğin son güncelleştirme zaman damgasını cihaz tarafından korur. Zaman damgaları UTC biçimindedir ve ıSO8601 biçiminde kodlanır YYYY-MM-DDTHH: MM: SS. mmmZ |
| `{componentName}` | Bir kök nesneye benzer şekilde bileşenin özellik değerlerini ve meta verilerini içeren bir JSON nesnesi. |
| `{componentName}.{propertyName}` | JSON 'daki bileşen özelliğinin değeri |
| `{componentName}.$metadata` | Bileşene ait meta veri bilgileri, kök düzeyine benzer`$metadata` |

#### <a name="device-twin-sample"></a>Cihaz Ikizi örneği

Aşağıdaki kod parçacığında, bir ikizi, JSON nesnesi olarak biçimlendirilen bir IoT Tak ve Kullan cihazı gösterilmektedir:

```json
{
    "deviceId": "sample-device",
    "modelId": "dtmi:com:example:TemperatureController;1",
    "version": 15,
    "properties": {
        "desired": {
            "thermostat1": {
                "__t": "c",
                "targetTemperature": 21.8
            },
            "$metadata": {...},
            "$version": 4
        },
        "reported": {
            "serialNumber": "alwinexlepaho8329",
            "thermostat1": {
                "maxTempSinceLastReboot": 25.3,
                "__t": "c",
                "targetTemperature": {
                    "value": 21.8,
                    "ac": 200,
                    "ad": "Successfully executed patch",
                }
            },
            "$metadata": {...},
            "$version": 11
        }
    }
}
```

#### <a name="digital-twin-sample"></a>Dijital Ikizi örneği

Aşağıdaki kod parçacığında, JSON nesnesi olarak biçimlendirilen dijital ikizi gösterilmektedir:

```json
{
    "$dtId": "sample-device",
    "serialNumber": "alwinexlepaho8329",
    "thermostat1": {
        "maxTempSinceLastReboot": 25.3,
        "targetTemperature": 21.8,
        "$metadata": {
            "targetTemperature": {
                "desiredValue": 21.8,
                "desiredVersion": 4,
                "ackVersion": 4,
                "ackCode": 200,
                "ackDescription": "Successfully executed patch",
                "lastUpdateTime": "2020-07-17T06:11:04.9309159Z"
            },
            "maxTempSinceLastReboot": {
                "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
            }
        }
    },
    "$metadata": {
        "$model": "dtmi:com:example:TemperatureController;1",
        "serialNumber": {
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

### <a name="properties"></a>Özellikler

Özellikler, bir varlığın durumunu temsil eden veri alanlarıdır (birçok nesne odaklı programlama dilinde özellikler gibi).

#### <a name="read-only-property"></a>Salt okunurdur özelliği

Manızı

```json
{
    "@type": "Property",
    "name": "serialNumber",
    "displayName": "Serial Number",
    "description": "Serial number of the device.",
    "schema": "string"
}
```

Bu örnekte, `alwinexlepaho8329` `serialNumber` cihaz tarafından bildirilen salt okunurdur özelliğinin geçerli değeridir.
Aşağıdaki kod parçacıkları, özelliğinin yan yana JSON gösterimini gösterir `serialNumber` :

:::row:::
   :::column span="":::
      **Cihaz çifti**

```json
"properties": {
    "reported": {
        "serialNumber": "alwinexlepaho8329"
    }
}
```

   :::column-end:::
   :::column span="":::
      **Dijital ikizi**

```json
"serialNumber": "alwinexlepaho8329"
```

   :::column-end:::
:::row-end:::

#### <a name="writable-property"></a>Yazılabilir Özellik

Ayrıca, cihaza kök düzeyinde aşağıdaki yazılabilir özelliği de vardı:

```json
{
    "@type": "Property",
    "name": "fanSpeed",
    "displayName": "Fan Speed",
    "writable": true,
    "schema": "double"
}
```

:::row:::
   :::column span="":::
      **Cihaz çifti**

```json
{
    "properties": {
        "desired": {
            "fanSpeed": 2.0,
        },
        "reported": {
            "fanSpeed": {
                "value": 3.0,
                "ac": 200,
                "av": 1,
                "ad": "Successfully executed patch version 1"
            }
        }
    },
}
```

   :::column-end:::
   :::column span="":::
      **Dijital ikizi**

```json
{
    "fanSpeed": 3.0,
    "$metadata": {
        "fanSpeed": {
            "desiredValue": 2.0,
            "desiredVersion": 2,
            "ackVersion": 1,
            "ackCode": 200,
            "ackDescription": "Successfully executed patch version 1",
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

   :::column-end:::
:::row-end:::

Bu örnekte, `3.0` `fanSpeed` cihaz tarafından bildirilen özelliğin geçerli değeridir. `2.0`, çözüm tarafından ayarlanan istenen değerdir. Kök düzeyindeki bir özelliğin istenen değeri ve eşitleme durumu, `$metadata` dijital ikizi için kök düzeyinde ayarlanır. Cihaz çevrimiçi olduğunda, bu güncelleştirmeyi uygulayabilir ve güncelleştirilmiş değeri geri bildirebilirsiniz.

### <a name="components"></a>Bileşenler

Bileşenler, model arabiriminin diğer arabirimlerin derlemesi olarak oluşturulmasına izin verir.
Model olarak tanımlanan [termostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) arabirimini göz önünde bulundurun.
Bu arabirim artık [ısı denetleyicisi modeli](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)tanımlarken bir bileşen thermostat1 (ve başka bir bileşen thermostat2) olarak eklenebilir.

Bir cihaz ikizi, bir bileşen işaret tarafından tanımlanır `{ "__t": "c"}` . Dijital bir ikizi, `$metadata` bir bileşeni işaret ediyor.

Bu örnekte, `thermostat1` iki özelliği olan bir bileşendir:

- `maxTempSinceLastReboot`salt okunurdur.
- `targetTemperature`, cihaz tarafından başarıyla eşitlenen yazılabilir bir özelliktir. Bu özelliklerin istenen değeri ve eşitleme durumu bileşen ' de bulunur `$metadata` .

Aşağıdaki kod parçacıkları, bileşenin yan yana JSON gösterimini gösterir `thermostat1` :

:::row:::
   :::column span="":::
      **Cihaz çifti**

```json
"properties": {
    "desired": {
        "thermostat1": {
            "__t": "c",
            "targetTemperature": 21.8
        },
        "$metadata": {
        },
        "$version": 4
    },
    "reported": {
        "thermostat1": {
            "maxTempSinceLastReboot": 25.3,
            "__t": "c",
            "targetTemperature": {
                "value": 21.8,
                "ac": 200,
                "ad": "Successfully executed patch",
                "av": 4
            }
        },
        "$metadata": {
        },
        "$version": 11
    }
}
```

   :::column-end:::
   :::column span="":::
      **Dijital ikizi**

```json
"thermostat1": {
    "maxTempSinceLastReboot": 25.3,
    "targetTemperature": 21.8,
    "$metadata": {
        "targetTemperature": {
            "desiredValue": 21.8,
            "desiredVersion": 4,
            "ackVersion": 4,
            "ackCode": 200,
            "ackDescription": "Successfully executed patch",
            "lastUpdateTime": "2020-07-17T06:11:04.9309159Z"
        },
        "maxTempSinceLastReboot": {
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

   :::column-end:::
:::row-end:::

## <a name="digital-twin-apis"></a>Dijital ikizi API 'Leri

Azure dijital TWINS, Device Digital ikizi 'yi yönetmek için **Digital ikizi**, **Update Digital Ikizi**, **Component komutunu çağır** ve **Invoke komutunu** kullanarak donatılmıştır. [REST API 'leri](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin) doğrudan ya da bir [hizmet SDK 'sı](../iot-pnp/libraries-sdks.md)aracılığıyla kullanabilirsiniz.

## <a name="digital-twin-change-events"></a>Dijital ikizi değişiklik olayları

Dijital ikizi değişiklik olayları etkinleştirildiğinde, bileşen veya özelliğin geçerli veya istenen değeri her değiştiğinde bir olay tetiklenir. Dijital ikizi değişiklik olayları [JSON Patch](http://jsonpatch.com/) biçiminde oluşturulur. İkizi değişiklik olayları etkinse, ilgili olaylar Device ikizi biçiminde oluşturulur.

Cihaz ve dijital ikizi olayları için yönlendirmeyi nasıl etkinleştireceğinizi öğrenmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events). İleti biçimini anlamak için bkz. [IoT Hub Iletileri oluşturma ve okuma](../iot-hub/iot-hub-devguide-messages-construct.md).

Örneğin, aşağıdaki Digital ikizi Change olayı `targetTemperature` çözüm tarafından ayarlandığında tetiklenir:

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/17/2020 6:11:04 AM
iothub-message-source:digitalTwinChangeEvents
correlation-id:275d463fa034
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature",
    "value": {
      "desiredValue": 21.8,
      "desiredVersion": 4
    }
  }
]
```

Aşağıdaki dijital ikizi değişiklik olayı, cihaz yukarıdaki istenen değişikliğin uygulandığını bildirdiğinde tetiklenir:

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/17/2020 6:11:05 AM
iothub-message-source:digitalTwinChangeEvents
correlation-id:275d464a2c80
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackCode",
    "value": 200
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackDescription",
    "value": "Successfully executed patch"
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackVersion",
    "value": 4
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/lastUpdateTime",
    "value": "2020-07-17T06:11:04.9309159Z"
  },
  {
    "op": "add",
    "path": "/thermostat1/targetTemperature",
    "value": 21.8
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Artık dijital TWINS hakkında bilgi edindiğinize göre, bazı ek kaynaklar aşağıda verilmiştir:

- [IoT Tak ve Kullan Digital ikizi API 'Lerini kullanma](howto-manage-digital-twin.md)
- [Çözümünüzden bir cihazla etkileşim kurma](quickstart-service-node.md)
- [IoT dijital Ikizi REST API](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT Gezgini](howto-use-iot-explorer.md)
