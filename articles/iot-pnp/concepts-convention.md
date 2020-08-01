---
title: IoT Tak ve Kullan kuralları | Microsoft Docs
description: IoT Tak ve Kullan kurallarının açıklaması, cihazların telemetri ve özellik gönderirken kullanmasını bekler ve komutları ve özellik güncelleştirmelerini işler.
author: rido-min
ms.author: rmpablos
ms.date: 07/10/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 126673391b49f884a51521d462060c425a314667
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475677"
---
# <a name="iot-plug-and-play-conventions"></a>IoT Tak Çalıştır kuralları

IoT Tak ve Kullan önizleme aygıtları, bir IoT Hub ile ileti alışverişi yaparken bir dizi kural izlemelidir. IoT Tak ve Kullan önizleme Cihazları IoT Hub iletişim kurmak için MQTT protokolünü kullanır.

IoT Tak ve Kullan cihazının [dijital bir TWINS tanım dili v2 (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) modeliyle uyguladığı telemetri, özellik ve komutları _betimleyebilirsiniz_. Bu makalede başvurulan iki tür model vardır:

- **Bileşen yok** -bileşeni olmayan bir model. Model, ana arabirimin içerikler bölümünde telemetri, özellik ve komutları en üst düzey özellikler olarak bildirir.
- **Birden çok bileşen** -iki veya daha fazla arabirimden oluşan bir model. Telemetri, Özellikler ve komutları içeren bir ana arabirim. Ek telemetri, özellik ve komutlarla bileşen olarak belirtilen bir veya daha fazla arabirim.

Daha fazla bilgi için bkz. [modellerdeki ıot Tak ve kullan bileşenleri](concepts-components.md).

## <a name="identify-the-model"></a>Modeli tanımla

Uyguladığı modeli duyurmak için bir IoT Tak ve Kullan cihazı, alanına ekleyerek MQTT bağlantı paketindeki model KIMLIĞINI içerir `model-id` `USERNAME` .

Bir cihazın uyguladığı modeli belirlemek için, bir hizmet şu kaynaktan model KIMLIĞINI alabilir:

- Device ikizi `modelId` alanı.
- Digital ikizi `$metadata.$model` alanı.
- Dijital ikizi değişiklik bildirimi.

## <a name="telemetry"></a>Telemetri

Bileşen olmayan bir cihazdan gönderilen telemetri hiçbir ek meta veri gerektirmez. Sistem, özelliği ekler `dt-dataschema` .

Birden çok bileşen cihazından gönderilen telemetri `$.sub` bir ileti özelliği olarak eklememelidir. Sistem, `dt-subject` ve özelliklerini ekler `dt-dataschema` .

## <a name="read-only-properties"></a>Salt okunurdur özellikleri

### <a name="sample-no-component-read-only-property"></a>Bileşen olmayan salt okuma özelliği örneği

Bir cihaz, DTDL v2 kurallarını izleyen geçerli bir JSON gönderebilir.

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:example: Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "temperature",
          "schema": "double"
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Örnek yük**

      ```json
      "reported" :
      {
        "temperature" : 21.3
      }
      ```
   :::column-end:::
:::row-end:::

### <a name="sample-multiple-components-read-only-property"></a>Örnek birden çok bileşen salt okunurdur özelliği

Cihazın, `{"__t": "c"}` öğenin bir bileşene başvurduğunu göstermek için işaretçiyi eklemesi gerekir.

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:TemperatureController;1",
      "@type": "Interface",
      "displayName": "Temperature Controller",
      "contents": [
        {
          "@type" : "Component",
          "schema": "dtmi:com:example:Thermostat;1",
          "name": "thermostat1"
        }
      ]

      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "temperature",
          "schema": "double"
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Bildirilen Özellik**

      ```json
      "reported": {
        "thermostat1": {
          "__t": "c",
          "temperature": 21.3
        }
      }
      ```
   :::column-end:::
:::row-end:::

## <a name="writable-properties"></a>Yazılabilir Özellikler

Cihaz bildirilen bir özellik göndererek özelliği aldığını onaylamasını sağlamalıdır. Bildirilen özellik şunları içermelidir:

- `value`-cihazın aldığı değer.
- `ac`-bir HTTP durum kodu kullanan bir bildirim kodu.
- `av`-istenen özelliğin öğesine başvuran bir bildirim sürümü `$version` .
- `ad`-isteğe bağlı bir bildirim açıklaması.

### <a name="sample-no-component-writable-property"></a>Örnek bileşen yazılabilir özelliği yok

Bir cihaz, DTDL v2 kurallarını izleyen geçerli bir JSON gönderebilir:

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:example: Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "targetTemperature",
          "schema": "double",
          "writable": true
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **İstenen özellik**

      ```json
      "desired" :
      {
        "targetTemperature" : 21.3
      },
      "$version" : 3
      ```
   :::column-end:::
   :::column span="":::
      **Bildirilen Özellik**

      ```json
      "reported": {
        "targetTemperature": {
          "value": 21.3,
          "ac": 200,
          "av": 3,
          "ad": "complete"
       }
     }
      ```
   :::column-end:::
:::row-end:::

### <a name="sample-multiple-components-writable-property"></a>Örnek birden çok bileşen yazılabilir özelliği

Cihazın, `{"__t": "c"}` öğenin bir bileşene başvurduğunu göstermek için işaretçiyi eklemesi gerekir.

İşaretleyici yalnızca bileşen düzeyi güncelleştirmeler için gönderilir, bu nedenle cihazlar bu bayrağı kontrol eder.

Cihaz bildirilen bir özellik göndererek özelliği aldığını onaylamasını sağlamalıdır:

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:TemperatureController;1",
      "@type": "Interface",
      "displayName": "Temperature Controller",
      "contents": [
        {
          "@type" : "Component",
          "schema": "dtmi:com:example:Thermostat;1",
          "name": "thermostat1"
        }
      ]

      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "targetTemperature",
          "schema": "double",
          "writable": true
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **İstenen özellik**

      ```json
      "desired": {
        "thermostat1": {
          "__t": "c",
          "targetTemperature": 21.3
        }
      },
      "$version" : 3
      ```
   :::column-end:::
   :::column span="":::
      **Bildirilen Özellik**

      ```json
      "reported": {
        "thermostat1": {
          "__t": "c",
          "targetTemperature": {
            "value": 23,
            "ac": 200,
            "av": 3,
            "ad": "complete"
          }
        }
      }
      ```
   :::column-end:::
:::row-end:::

## <a name="commands"></a>Komutlar

Hiçbir bileşen arabirimi, önek olmadan komut adını kullanmaz.

Bir cihazda, birden çok bileşen arabirimi komut adlarını aşağıdaki biçimde kullanır: `componentName*commandName` .

## <a name="next-steps"></a>Sonraki adımlar

IoT Tak ve Kullan kuralları hakkında bilgi edindiğinize göre artık bazı ek kaynaklar verilmiştir:

- [Dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C cihaz SDK’sı](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](https://docs.microsoft.com/rest/api/iothub/device)
- [Model bileşenleri](./concepts-components.md)
