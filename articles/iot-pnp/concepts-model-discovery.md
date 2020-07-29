---
title: IoT Tak ve Kullan önizleme modeli bulmayı uygulama | Microsoft Docs
description: Çözüm Oluşturucu olarak, çözümünüzde IoT Tak ve Kullan modeli bulmayı nasıl uygulayabileceğinizi öğrenin.
author: prashmo
ms.author: prashmo
ms.date: 07/23/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 364b85a8ead09858b97d5d7e6ca8c130b9960b2c
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337390"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>IoT çözümünde IoT Tak ve Kullan önizleme modeli bulmayı uygulama

Bu makalede, bir IoT çözümünde IoT Tak ve Kullan önizleme modeli bulmayı nasıl uygulayabileceğinizi anlatan bir çözüm Oluşturucu olarak açıklanmaktadır. Model bulma şunları açıklar:

- IoT Tak ve Kullan cihazları, model KIMLIKLERINI kaydeder.
- IoT çözümü, cihaz tarafından uygulanan arabirimleri alır.

IoT çözümünün iki geniş kategorisi vardır:

- *Amaç oluşturulmuş bir IoT çözümü* , bilinen bir IoT Tak ve kullan cihaz modeli kümesiyle birlikte çalışmaktadır.

- *Model temelli bir IoT çözümü* , tüm IoT Tak ve kullan cihazlarla çalışabilir. Model temelli bir çözüm oluşturmak daha karmaşıktır, ancak çözümünüz gelecekte eklenen tüm cihazlarla birlikte çalışmıdır.

    Model temelli bir IoT çözümü oluşturmak için IoT Tak ve Kullan arabirimi temel elemanlarına karşı mantık oluşturmanız gerekir: telemetri, Özellikler ve komutlar. Çözümünüzün mantığı birden çok telemetri, özellik ve komut özelliklerini birleştirerek bir cihazı temsil eder.

Bu makalede her iki çözüm türünde model bulmanın nasıl uygulanacağı açıklanır.

## <a name="model-discovery"></a>Model keşfi

Bir cihazın uyguladığı modeli bulmak için, bir çözüm, olay tabanlı bulma veya ikizi tabanlı bulma kullanarak model KIMLIĞINI alabilir:

### <a name="event-based-discovery"></a>Olay tabanlı bulma

IoT Tak ve Kullan bir cihaz IoT Hub bağlandığı zaman, onun uyguladığı modeli kaydeder. Bu kayıt, [dijital ikizi değişiklik olay](concepts-digital-twin.md#digital-twin-change-events) bildirimine neden olur. Dijital ikizi olayları için yönlendirmeyi nasıl etkinleştireceğinizi öğrenmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Çözüm, aşağıdaki kod parçacığında gösterilen olayı kullanarak bağlantı KIMLIĞINI bağlayan ve aldığı IoT Tak ve Kullan cihaz hakkında bilgi alabilir:

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/22/2020 8:02:27 PM
iothub-message-source:digitalTwinChangeEvents
correlation-id:100f322dc2c5
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:example:TemperatureController;1"
  }
]
```

Bu olay, cihaz modeli KIMLIĞI eklendiğinde veya güncelleştirilirken tetiklenir.

### <a name="twin-based-discovery"></a>İkizi tabanlı bulma

Çözüm, belirli bir cihazın yeteneklerini öğrenmek isterse, bu bilgileri almak için [Get Digital ikizi](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin/getdigitaltwin) API 'sini kullanabilir.

Aşağıdaki dijital ikizi kod parçacığında `$metadata.$model` ıot Tak ve kullan cihazının model kimliğini içerir:

```json
{
    "$dtId": "sample-device",
    "$metadata": {
        "$model": "dtmi:com:example:TemperatureController;1",
        "serialNumber": {
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

Çözüm, aşağıdaki kod parçacığında gösterildiği gibi cihaz ikizi 'dan model KIMLIĞINI almak için **Get ikizi** de kullanabilir:

```json
{
    "deviceId": "sample-device",
    "etag": "AAAAAAAAAAc=",
    "deviceEtag": "NTk0ODUyODgx",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00Z",
    "connectionState": "Disconnected",
    "lastActivityTime": "2020-07-17T06:12:26.8402249Z",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "modelId": "dtmi:com:example:TemperatureController;1",
    "version": 15,
    "properties": {...}
    }
}
```

## <a name="model-resolution"></a>Model çözümleme

Bir çözüm model KIMLIĞINDEN model oluşturan arabirimlere erişim sağlamak için model çözümleme kullanır. 

- Çözümler, bu arabirimleri yerel bir klasörde dosya olarak depolamayı tercih edebilir. 
- Çözümler [model deposunu](concepts-model-repository.md)kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir IoT çözümünü model bulma hakkında öğrendiğinize göre, çözümünüz için diğer özellikleri kullanmak üzere [Azure IoT Platformu](overview-iot-plug-and-play.md) hakkında daha fazla bilgi edinin.

- [Çözümünüzden bir cihazla etkileşim kurma](quickstart-service-node.md)
- [IoT dijital Ikizi REST API](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT Gezgini](howto-use-iot-explorer.md)
