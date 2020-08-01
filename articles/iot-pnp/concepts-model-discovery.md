---
title: Bir çözümde IoT Tak ve Kullan modellerini kullanma | Microsoft Docs
description: Bir çözüm Oluşturucusu olarak IoT çözümünüzde IoT Tak ve Kullan modellerini nasıl kullanabileceğinizi öğrenin.
author: arunmannengal
ms.author: arunmann
ms.date: 07/23/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 4cdd6f63c9e5e717a533b88702b2886387fe3e39
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475252"
---
# <a name="use-iot-plug-and-play-models-in-an-iot-solution"></a>IoT çözümünde IoT Tak ve Kullan modellerini kullanma

Bu makalede, IoT çözümünde bir IoT Tak ve Kullan cihazının model KIMLIĞINI tanımlayabilir ve sonra model tanımını alabileceğinizi açıklanmaktadır.

IoT çözümünün iki geniş kapsamlı kategorisi vardır:

- *Amaç oluşturulmuş bir çözüm* , çözüme bağlanacak IoT Tak ve kullan cihazları için bilinen bir model kümesiyle çalışır. Çözümü geliştirirken bu modelleri kullanırsınız.

- *Model temelli* bir çözüm, herhangi bir IoT Tak ve kullan cihazının modeliyle çalışabilir. Model temelli bir çözüm oluşturmak daha karmaşıktır, ancak çözümünüz gelecekte eklenebilecek tüm cihazlarla birlikte çalışmıdır. Model temelli bir IoT çözümü modeli alır ve cihazın uyguladığı telemetri, özellik ve komutları belirlemede kullanır.

IoT Tak ve Kullan modelini kullanmak için bir IoT çözümü:

1. Çözüme bağlı IoT Tak ve Kullan cihazı tarafından uygulanan modelin model KIMLIĞINI tanımlar.

1. Model deposundan veya özel depodan bağlı cihazın model tanımını almak için model KIMLIĞINI kullanır.

## <a name="identify-model-id"></a>Model KIMLIĞINI tanımla

IoT Tak ve Kullan bir cihaz IoT Hub bağlandığı zaman, IoT Hub uyguladığı modelin model KIMLIĞINI kaydeder.

IoT Hub cihaz bağlantı akışının bir parçası olarak çözümü cihaz modeli KIMLIĞIYLE bilgilendirir.

Bir çözüm, aşağıdaki üç yöntemden birini kullanarak IoT Tak ve Kullan cihazının model KIMLIĞINI alabilir:

### <a name="get-device-twin-api"></a>Cihaz Ikizi API 'sini al

Çözüm, IoT Tak ve Kullan cihazının model KIMLIĞINI almak için [Device ikizi](https://docs.microsoft.com/rest/api/iothub/service/twin/getdevicetwin) API 'sini alabilir.

Aşağıdaki cihaz ikizi yanıt kod parçacığında, `modelId` ıot Tak ve kullan cihazının model kimliğini içerir:

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

### <a name="get-digital-twin-api"></a>Digital Ikizi API 'sini al

Çözüm, IoT Tak ve Kullan cihazı tarafından uygulanan modelin model KIMLIĞINI almak için [Get Digital ikizi](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin/getdigitaltwin) API 'sini kullanabilir.

Aşağıdaki Digital ikizi yanıt kod parçacığında, `$metadata.$model` ıot Tak ve kullan cihazının model kimliğini içerir:

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

### <a name="digital-twin-change-event-notification"></a>Dijital ikizi değişiklik olayı bildirimi

Bir cihaz bağlantısı, [dijital ikizi değişiklik olay](concepts-digital-twin.md#digital-twin-change-events) bildirimine neden olur. Bir çözümün bu olay bildirimine abone olması gerekir. Dijital ikizi olayları için yönlendirmeyi nasıl etkinleştireceğinizi öğrenmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Çözüm, şu kod parçacığında gösterilen olayı kullanarak bağlantı KIMLIĞINI bağlayan IoT Tak ve Kullan cihazı hakkında bilgi alabilir:

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

## <a name="retrieve-a-model-definition"></a>Model tanımı alma

Bir çözüm, karşılık gelen model tanımını almak için yukarıda tanımlanan model KIMLIĞINI kullanır.

Bir çözüm, aşağıdaki seçeneklerden birini kullanarak model tanımını alabilir:

### <a name="model-repository"></a>Model deposu

Çözümler modelleri almak için [model deposunu](concepts-model-repository.md) kullanabilir. Çözümün onları alabilmesi için cihaz oluşturucular veya çözüm üreticileri, modellerini depoya önceden yüklemelisiniz.

Yeni bir cihaz bağlantısı için model KIMLIĞINI tanımladıktan sonra aşağıdaki adımları izleyin:

1. Model deposundaki model KIMLIĞINI kullanarak model tanımını alın. Daha fazla bilgi için bkz. [modelleri al](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync).

1. Bağlı cihazın model tanımını kullanarak, cihazın yeteneklerini sıralayabilirsiniz.

1. Cihazın numaralandırılmış yeteneklerini kullanarak, kullanıcıların [cihazla etkileşime](quickstart-service-node.md)girmesine izin verebilirsiniz.

### <a name="custom-store"></a>Özel mağaza

Çözümler, bu model tanımlarını bir yerel dosya sisteminde, ortak bir dosya deposunda saklayabilir veya özel bir uygulama kullanabilir.

Yeni bir cihaz bağlantısı için model KIMLIĞINI tanımladıktan sonra aşağıdaki adımları izleyin:

1. Özel deponuzdaki model KIMLIĞINI kullanarak model tanımını alın.

1. Bağlı cihazın model tanımını kullanarak, cihazın yeteneklerini sıralayabilirsiniz. 

1. Cihazın numaralandırılmış yeteneklerini kullanarak, kullanıcıların [cihazla etkileşime](quickstart-service-node.md)girmesine izin verebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

IoT çözümünde IoT Tak ve Kullan modellerini nasıl tümleştirileceğini öğrendiğinize göre, önerilen bazı sonraki adımlar şunlardır:

- [Çözümünüzden bir cihazla etkileşim kurma](quickstart-service-node.md)
- [IoT dijital Ikizi REST API](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT Gezgini](howto-use-iot-explorer.md)
