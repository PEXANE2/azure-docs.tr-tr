---
title: Uzaktan izleme çözümünde cihaz şeması-Azure | Microsoft Docs
description: Bu makalede, uzaktan izleme çözümünde sanal bir cihazı tanımlayan JSON şeması açıklanmaktadır.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: ac681bb13ccea49c7a2f566a6fcdb6adb8cec5bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81683739"
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şemasını anlama

Davranışını test etmek için uzaktan Izleme çözümünde sanal cihazları kullanabilirsiniz. Uzaktan Izleme çözümü, sanal cihazları çalıştırmak için bir cihaz benzetimi hizmeti içerir. Uzaktan Izleme çözümünü dağıttığınızda, sanal cihazların bir koleksiyonu otomatik olarak sağlanır. Var olan sanal cihazları özelleştirebilir veya kendi oluşturduğunuz cihazları oluşturabilirsiniz.

Bu makalede, sanal bir cihazın yeteneklerini ve davranışını belirten cihaz modeli şeması açıklanmaktadır. Cihaz modeli bir JSON dosyasında depolanır.

> [!NOTE]
> Bu cihaz modeli şeması yalnızca cihaz benzetimi hizmetinde barındırılan sanal cihazlar içindir. Gerçek bir cihaz oluşturmak istiyorsanız, bkz. [cihazınızı uzaktan izleme çözüm hızlandırıcısına bağlama](iot-accelerators-connecting-devices.md).

Aşağıdaki makaleler geçerli makaleyle ilgilidir:

* [Cihaz modeli davranışını uygulamak](iot-accelerators-remote-monitoring-device-behavior.md) , sanal bir cihazın davranışını uygulamak Için kullandığınız JavaScript dosyalarını açıklar.
* [Yeni bir sanal cihaz oluşturma, bir](iot-accelerators-remote-monitoring-create-simulated-device.md) araya getirir ve çözümünüze yeni bir sanal cihaz türü dağıtmayı gösterir.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Sanal cihaz modeli tanımlamak için JSON dosyası kullanma
> * Sanal cihazın özelliklerini belirtin
> * Sanal cihazın gönderdiği Telemetriyi belirtin
> * Cihazın yanıt verdiği buluttan cihaza yöntemleri belirtin

## <a name="the-parts-of-the-device-model-schema"></a>Cihaz modeli şemasının bölümleri

Bir chilya da kamyon gibi her bir cihaz modeli benzetim hizmetinin benzetimini gerçekleştirebileceği bir cihaz türü tanımlar. Her cihaz modeli, aşağıdaki üst düzey şemaya sahip bir JSON dosyasında depolanır:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

GitHub 'daki [devicemodeller klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) varsayılan sanal cihazların şema dosyalarını görüntüleyebilirsiniz.

Aşağıdaki tabloda en üst düzey şema girdileri açıklanmaktadır:

| Şema girdisi | Açıklama |
| -- | --- |
| `SchemaVersion` | Şema sürümü her zaman `1.0.0` ve bu dosyanın biçimine özgüdür. |
| `Id` | Bu cihaz modeli için benzersiz bir KIMLIK. |
| `Version` | Cihaz modelinin sürümünü tanımlar. |
| `Name` | Cihaz modeli için kolay bir ad. |
| `Description` | Cihaz modelinin açıklaması. |
| `Protocol` | Cihazın kullandığı bağlantı protokolü. `AMQP`, `MQTT`Ve `HTTP`' den biri olabilir. |

Aşağıdaki bölümlerde JSON şemasındaki diğer bölümler açıklanır:

## <a name="simulation"></a>Benzetim

`Simulation` Bölümünde, sanal cihazın iç durumunu tanımlarsınız. Cihaz tarafından gönderilen telemetri değerlerinin bu cihaz durumunun bir parçası olması gerekir.

Cihaz durumunun tanımında iki öğe vardır:

* `InitialState`Cihaz durumu nesnesinin tüm özellikleri için başlangıç değerlerini tanımlar.
* `Script`cihaz durumunu güncelleştirmek için bir zamanlamaya göre çalışan bir JavaScript dosyasını tanımlar. Cihaz tarafından gönderilen telemetri değerlerini rastgele atamak için bu komut dosyasını kullanabilirsiniz.

Cihaz durumu nesnesini güncelleştiren JavaScript dosyası hakkında daha fazla bilgi edinmek için bkz. [cihaz modeli davranışını anlama](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md).

Aşağıdaki örnekte, sanal bir chilcihaz için cihaz durumu nesnesinin tanımı gösterilmektedir:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

Simülasyon hizmeti, cihaz durumunu güncelleştirmek için her beş saniyede bir **Chiller-01-State. js** dosyasını çalıştırır. GitHub 'daki [betikler klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) varsayılan sanal cihazların JavaScript dosyalarını görebilirsiniz. Kurala göre, bu JavaScript dosyaları Yöntem davranışlarını uygulayan dosyalardan ayırt edilebilmesi için sonek **durumundadır** .

## <a name="properties"></a>Özellikler

Şemanın `Properties` bölümü, cihazın çözüme rapor bildirdiği özellik değerlerini tanımlar. Örneğin:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Çözüm başladığında, Kullanıcı arabiriminde kullanılacak `Type` değerlerin bir listesini oluşturmak için tüm sanal cihazları sorgular. Çözüm, `Latitude` Pano 'daki haritaya `Longitude` cihaz konumunu eklemek için ve özelliklerini kullanır.

## <a name="telemetry"></a>Telemetri

`Telemetry` Dizi, sanal cihazın çözüme gönderdiği tüm telemetri türlerini listeler.

Aşağıdaki örnek `floor`, `vibration`, ve `temperature` Asansör sensörlerinden alınan verileri her 10 saniyede bir JSON telemetri iletisi gönderir:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate`sanal cihaz tarafından gönderilen JSON iletisinin yapısını tanımlar. İçindeki `MessageTemplate` yer tutucular, `${NAME}` [cihaz durumu nesnesinden](#simulation)bir anahtar `NAME` olan söz dizimini kullanır. Dizeler tırnak içine alınmalıdır, sayılar olmamalıdır.

`MessageSchema`sanal cihaz tarafından gönderilen iletinin şemasını tanımlar. İleti şeması, arka uç uygulamalarının gelen telemetrileri yorumlamak için bilgileri yeniden kullanmasına olanak tanımak üzere IoT Hub de yayımlanır.

Şu anda yalnızca JSON ileti şemalarını kullanabilirsiniz. Şemada listelenen alanlar aşağıdaki türlerde olabilir:

* Nesne-JSON kullanılarak serileştirilmiş
* Base64 kullanılarak ikili seri hale getirilmiş
* Metin
* Boole
* Tamsayı
* Çift
* DateTime

Farklı aralıklarda telemetri iletileri göndermek için, `Telemetry` diziye birden çok telemetri türü ekleyin. Aşağıdaki örnek, her 10 saniyede bir sıcaklık ve nem verisi ve her dakika ışığın durumunu gönderir:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate":
      "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
    "MessageSchema": {
      "Name": "RoomComfort;v1",
      "Format": "JSON",
      "Fields": {
        "temperature": "double",
        "temperature_unit": "text",
        "humidity": "integer"
      }
    }
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Sanal cihaz, bir IoT Hub 'ından çağrılan buluttan cihaza yöntemlere yanıt verebilir. Cihaz `CloudToDeviceMethods` modeli şema dosyasındaki bölümü:

* Sanal cihazın yanıt verebildiği yöntemleri tanımlar.
* Yürütülecek mantığı içeren JavaScript dosyasını tanımlar.

Sanal cihaz, bağlandığı IoT Hub 'ına desteklediği yöntemlerin listesini gönderir.

Cihazın davranışını uygulayan JavaScript dosyası hakkında daha fazla bilgi edinmek için bkz. [cihaz modeli davranışını anlama](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md).

Aşağıdaki örnek, üç desteklenen yöntemi ve bu yöntemleri uygulayan JavaScript dosyalarını belirtir:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

GitHub 'daki [betikler klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) varsayılan sanal cihazların JavaScript dosyalarını görebilirsiniz. Kurala göre, bu JavaScript dosyalarının, durum davranışını uygulayan dosyalardan ayırt edilebilmesi için sonek **-yöntemi** vardır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kendi özel sanal cihaz modelinizi oluşturma açıklanır. Bu makalede nasıl yapılacağı açıklanır:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Sanal cihaz modeli tanımlamak için JSON dosyası kullanma
> * Sanal cihazın özelliklerini belirtin
> * Sanal cihazın gönderdiği Telemetriyi belirtin
> * Cihazın yanıt verdiği buluttan cihaza yöntemleri belirtin

JSON şeması hakkında öğrendiğinize göre, önerilen sonraki adım, [sanal cihazınızın davranışını nasıl uygulayacağınızı](iot-accelerators-remote-monitoring-device-behavior.md)öğrenmelidir.

Uzaktan Izleme çözümü hakkında daha fazla geliştirici bilgisi için bkz.:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici sorun giderme kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
