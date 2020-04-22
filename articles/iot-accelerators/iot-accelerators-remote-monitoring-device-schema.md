---
title: Uzaktan izleme çözümünde aygıt şeması - Azure | Microsoft Dokümanlar
description: Bu makalede, uzaktan izleme çözümünde simüle edilmiş bir aygıtı tanımlayan JSON şeması açıklanmaktadır.
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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683739"
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şemasını anlama

Davranışını sınamak için Uzaktan İzleme çözümünde simüle edilmiş aygıtlar kullanabilirsiniz. Uzaktan İzleme çözümü, simüle edilmiş aygıtları çalıştırmak için bir aygıt simülasyon hizmeti içerir. Uzaktan İzleme çözümünü dağıttığınızda, simüle edilmiş aygıtlar koleksiyonu otomatik olarak karşılanır. Varolan simüle edilmiş aygıtları özelleştirebilir veya kendi aygıtlarınızı oluşturabilirsiniz.

Bu makalede, benzetilen bir aygıtın yeteneklerini ve davranışlarını belirten aygıt modeli şeması açıklanmaktadır. Aygıt modeli bir JSON dosyasında depolanır.

> [!NOTE]
> Bu cihaz modeli şeması yalnızca aygıt simülasyon hizmetinde barındırılan simüle edilmiş aygıtlar içindir. Gerçek bir aygıt oluşturmak istiyorsanız, [cihazınızı Uzaktan İzleme çözüm hızlandırıcısına bağlayın'](iot-accelerators-connecting-devices.md)a bakın.

Aşağıdaki makaleler geçerli makale ile ilgilidir:

* [Aygıt modeli davranışını uygulayın,](iot-accelerators-remote-monitoring-device-behavior.md) benzetimli aygıtın davranışını uygulamak için kullandığınız JavaScript dosyalarını açıklar.
* [Yeni bir simüle aygıt oluşturun,](iot-accelerators-remote-monitoring-create-simulated-device.md) her şeyi bir araya getirir ve çözümünüze yeni bir simüle aygıt türünü nasıl dağıtabileceğinizi gösterir.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Benzetimli aygıt modelini tanımlamak için JSON dosyası kullanma
> * Benzetimli aygıtın özelliklerini belirtin
> * Simüle cihazın gönderdiği telemetriyi belirtin
> * Aygıtın yanıt olarak yanıtlediği bulut-aygıt yöntemlerini belirtin

## <a name="the-parts-of-the-device-model-schema"></a>Cihaz modeli şemasının parçaları

Chiller veya kamyon gibi her aygıt modeli, simülasyon hizmetinin simüle edebileceği bir aygıt türünü tanımlar. Her aygıt modeli, aşağıdaki üst düzey şema içeren bir JSON dosyasında depolanır:

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

GitHub'daki [aygıt modelleri klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) varsayılan benzetilen aygıtlar için şema dosyalarını görüntüleyebilirsiniz.

Aşağıdaki tabloda üst düzey şema girişleri açıklanmaktadır:

| Şema girişi | Açıklama |
| -- | --- |
| `SchemaVersion` | Şema sürümü her `1.0.0` zaman ve bu dosyanın biçimine özgüdür. |
| `Id` | Bu aygıt modeli için benzersiz bir kimlik. |
| `Version` | Aygıt modelinin sürümünü tanımlar. |
| `Name` | Cihaz modeli için uygun bir ad. |
| `Description` | Aygıt modelinin açıklaması. |
| `Protocol` | Aygıtın kullandığı bağlantı protokolü. Biri olabilir `AMQP`, `MQTT`ve `HTTP`. |

Aşağıdaki bölümlerde JSON şemasındaki diğer bölümler açıklayınız:

## <a name="simulation"></a>Benzetim

`Simulation` Bölümde, benzetilen aygıtın iç durumunu tanımlarsınız. Aygıt tarafından gönderilen herhangi bir telemetri değerleri bu aygıt durumunun bir parçası olmalıdır.

Aygıt durumunun tanımı iki öğeden oluşur:

* `InitialState`aygıt durumu nesnesinin tüm özellikleri için ilk değerleri tanımlar.
* `Script`aygıt durumunu güncelleştirmek için bir zamanlamada çalışan bir JavaScript dosyasını tanımlar. Aygıt tarafından gönderilen telemetri değerlerini rasgeleleştirmek için bu komut dosyası dosyasını kullanabilirsiniz.

Aygıt durumu nesnesini güncelleyen JavaScript dosyası hakkında daha fazla bilgi edinmek için [bkz.](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md)

Aşağıdaki örnek, benzetilen bir soğutucu aygıtı için aygıt durumu nesnesinin tanımını gösterir:

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

Simülasyon hizmeti, aygıt durumunu güncelleştirmek için **chiller-01-state.js** dosyasını her beş saniyede bir çalıştırıyor. GitHub'daki [komut dosyaları klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) varsayılan benzetilen aygıtlar için JavaScript dosyalarını görebilirsiniz. Bu JavaScript dosyaları, kural kuralına göre, yöntem davranışlarını uygulayan dosyalardan ayırt etmek için sonek **-duruma** sahiptir.

## <a name="properties"></a>Özellikler

Şema `Properties` bölümü, aygıt çözüme rapor edilen özellik değerlerini tanımlar. Örneğin:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Çözüm başlatıldığında, ui'de kullanılacak `Type` değerlerin bir listesini oluşturmak için tüm benzetilen aygıtları sorgular. Çözüm, aygıtın konumunu panodaki haritaya eklemek için özellikleri `Latitude` ve `Longitude` özelliklerini kullanır.

## <a name="telemetry"></a>Telemetri

Dizi, `Telemetry` simüle edilen aygıtın çözüme gönderdiği tüm telemetri türlerini listeler.

Aşağıdaki örnek, asansör sensörlerinden gelen verileri içeren `floor` `vibration` `temperature` her 10 saniyede bir JSON telemetri mesajı gönderir:

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

`MessageTemplate`benzetilen aygıt tarafından gönderilen JSON iletisinin yapısını tanımlar. Yer tutucular, `MessageTemplate` aygıt `${NAME}` durumu `NAME` [nesnesinden](#simulation)bir anahtar olan sözdizimini kullanır. Dizeleri alıntı yapılmalıdır, sayılar olmamalıdır.

`MessageSchema`benzetimli aygıt tarafından gönderilen iletinin şema tanımlar. Mesaj şeması, arka uç uygulamalarının gelen telemetriyi yorumlamak için bilgileri yeniden kullanmasını sağlamak için IoT Hub'a da yayınlanır.

Şu anda yalnızca JSON ileti şemalarını kullanabilirsiniz. Şemada listelenen alanlar aşağıdaki türlerden olabilir:

* Nesne - JSON kullanılarak seri hale getir
* İkili - base64 kullanılarak serihale
* Metin
* Boole
* Tamsayı
* Çift
* DateTime

Farklı aralıklarla telemetri iletileri göndermek için `Telemetry` diziye birden çok telemetri türü ekleyin. Aşağıdaki örnek, her 10 saniyede bir sıcaklık ve nem verilerini ve her dakika ışığın durumunu gönderir:

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

## <a name="cloudtodevicemethods"></a>CloudtoDevice Yöntemleri

Benzetimli bir aygıt, bir IoT hub'ından çağrılan buluttan aygıta yöntemlere yanıt verebilir. Aygıt `CloudToDeviceMethods` modeli şema dosyasındaki bölüm:

* Benzetimli aygıtın yanıt verebileceği yöntemleri tanımlar.
* Yürütülecek mantığı içeren JavaScript dosyasını tanımlar.

Benzetimli aygıt, desteklediği yöntemlerin listesini bağlı olduğu IoT hub'ına gönderir.

Aygıtın davranışını uygulayan JavaScript dosyası hakkında daha fazla bilgi edinmek için [bkz.](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md)

Aşağıdaki örnekte, desteklenen üç yöntem ve bu yöntemleri uygulayan JavaScript dosyaları belirtilir:

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

GitHub'daki [komut dosyaları klasöründe](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) varsayılan benzetilen aygıtlar için JavaScript dosyalarını görebilirsiniz. Bu JavaScript dosyaları, kural kuralına göre, bunları durum davranışı uygulayan dosyalardan ayırt etmek için sonek **yöntemine** sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kendi özel simüle aygıt modelinizi nasıl oluşturabilirsiniz açıklanmıştır. Bu makalede, nasıl gösterin:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Benzetimli aygıt modelini tanımlamak için JSON dosyası kullanma
> * Benzetimli aygıtın özelliklerini belirtin
> * Simüle cihazın gönderdiği telemetriyi belirtin
> * Aygıtın yanıt olarak yanıtlediği bulut-aygıt yöntemlerini belirtin

Şimdi JSON şema hakkında öğrendim, önerilen bir sonraki adım nasıl [simüle cihazın davranışını uygulamak](iot-accelerators-remote-monitoring-device-behavior.md)için öğrenmektir.

Uzaktan İzleme çözümü hakkında daha fazla geliştirici bilgisi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
