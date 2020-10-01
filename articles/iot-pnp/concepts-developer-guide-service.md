---
title: Hizmet Geliştirici Kılavuzu-IoT Tak ve Kullan | Microsoft Docs
description: Hizmet geliştiricileri için IoT Tak ve Kullan açıklaması
author: dominicbetts
ms.author: dobett
ms.date: 09/24/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: f082e4d4c6c71e460842f80a5aa17130b6a41279
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91614232"
---
# <a name="iot-plug-and-play-service-developer-guide"></a>IoT Tak ve Kullan hizmeti Geliştirici Kılavuzu

IoT Tak ve Kullan, yeteneklerini Azure IoT uygulamalarına duyuran akıllı cihazlar oluşturmanızı sağlar. IoT Tak ve Kullan cihazları, bir müşteri bunları IoT Tak ve Kullan özellikli uygulamalara bağladığında el ile yapılandırma gerektirmez.

IoT Tak ve Kullan, IoT Hub 'ınız ile model KIMLIĞINI duyurduğu cihazları kullanmanıza olanak sağlar. Örneğin, bir cihazın özelliklerine ve komutlarına doğrudan erişebilirsiniz.

IoT Hub 'ınıza bağlı bir IoT Tak ve Kullan cihazı kullanmak için, IoT hizmeti SDK 'Lardan birini veya IoT Hub REST API kullanın:

## <a name="service-sdks"></a>Hizmet SDK'ları

Cihazlarla ve modüllerle etkileşim kurmak için çözümünüzdeki Azure IoT hizmeti SDK 'larını kullanın. Örneğin, ikizi özelliklerini okumak ve güncelleştirmek ve komutları çağırmak için hizmet SDK 'larını kullanabilirsiniz. Desteklenen diller C#, Java, Node.js ve Python içerir.

Hizmet SDK 'Ları, masaüstü veya Web uygulaması gibi bir çözümden cihaz bilgilerine erişmenizi sağlar. Hizmet SDK 'Ları, model KIMLIĞINI almak için kullanabileceğiniz iki ad alanı ve nesne modeli içerir:

- IoT Hub hizmeti istemcisi. Bu hizmet, model KIMLIĞINI bir Device ikizi özelliği olarak kullanıma sunar.

- Dijital TWINS hizmeti istemcisi. Yeni dijital TWINS API 'SI, bir dijital TWINS tanım dili modeli tanımlanmış bileşenler, Özellikler ve komutlar gibi üst düzey yapılar üzerinde çalışır. Dijital Ikizi API 'Leri, çözüm oluşturucuların IoT Tak ve Kullan çözümleri oluşturmalarına daha kolay hale getirir.

| Platform | IoT Hub hizmeti istemcisi | Dijital TWINS hizmet istemcisi |
| -------- | ---------------------- | ---------------------------- |
| .NET     | [Belgeler](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.shared.twin.modelid?view=azure-dotnet#Microsoft_Azure_Devices_Shared_Twin_ModelId&preserve-view=true) <br/> [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/PnpServiceSamples)| [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/DigitalTwinClientSamples) |
| Java     | [Belgeler](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.devicetwin.devicetwindevice?view=azure-java-stable&preserve-view=true) <br/> [Örnekler](https://github.com/Azure/azure-iot-sdk-java/blob/master/service/iot-service-samples/pnp-service-sample)| [Örnekler](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/digitaltwin-service-samples) |
| Node.js  | [Belgeler](https://docs.microsoft.com/javascript/api/azure-iothub/twin?view=azure-node-latest&preserve-view=true) <br/> [Örnek](https://github.com/Azure/azure-iot-sdk-node/blob/master/service/samples/javascript/twin.js)| [Belgeler](https://docs.microsoft.com/javascript/api/azure-iot-digitaltwins-service/?view=azure-node-latest&preserve-view=true) <br/> [Örnek](https://github.com/Azure/azure-iot-sdk-node/blob/master/service/samples/javascript/get_digital_twin.js) |
| Python   | [Belgeler](https://docs.microsoft.com/python/api/azure-iot-hub/azure.iot.hub.iothubregistrymanager?view=azure-python&preserve-view=true) <br/> [Örnek](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/iothub_registry_manager_method_sample.py)| [Belgeler](https://docs.microsoft.com/python/api/azure-iot-hub/azure.iot.hub.iothubdigitaltwinmanager?view=azure-python&preserve-view=true) <br/> [Örnek](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/get_digital_twin_sample.py) |

## <a name="rest-api"></a>REST API

Aşağıdaki örneklerde, bağlı bir IoT Tak ve Kullan cihazından etkileşimde bulunmak için IoT Hub REST API kullanılmaktadır. API 'nin geçerli sürümü `2020-09-30` . `?api-version=2020-09-30`Rest PI çağrılarınızın sonuna ekleyin.

> [!NOTE]
> Modül TWINS 'leri Şu anda API tarafından desteklenmiyor `digitalTwins` .

Termostat cihazınız çağrılırsa `t-123` , REST API Get çağrısıyla cihazınız tarafından uygulanan tüm arabirimlerin tüm özelliklerini alırsınız:

```REST
GET /digitalTwins/t-123
```

Bu çağrıda, `$metadata.$model` cihaz tarafından duyurulan model kimliğiyle JSON özelliği dahil edilir.

Tüm arabirimlerde tüm özelliklere, `GET /DigitalTwin/{device-id}` cihazın tanımlayıcısı olan REST API şablonuyla erişilir `{device-id}` :

```REST
GET /digitalTwins/{device-id}
```

IoT Tak ve Kullan cihaz komutlarını doğrudan çağırabilirsiniz. `Thermostat` `t-123` Cihazdaki bileşen bir `restart` komuta sahipse, bunu bir REST API Post çağrısıyla çağırabilirsiniz:

```REST
POST /digitalTwins/t-123/components/Thermostat/commands/restart
```

Genel olarak, bu REST API şablonu aracılığıyla komutlar çağrılabilir:

- `device-id`: cihaz için tanımlayıcı.
- `component-name`: cihaz yetenek modelindeki Implements bölümünde arabirimin adı.
- `command-name`: komutun adı.

```REST
/digitalTwins/{device-id}/components/{component-name}/commands/{command-name}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz modelleme hakkında bilgi edindiğinize göre, bazı ek kaynaklar aşağıda verilmiştir:

- [Dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C cihaz SDK’sı](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](https://docs.microsoft.com/rest/api/iothub/device)
- [Model bileşenleri](./concepts-components.md)
- [DTDL yazma araçlarını yükle ve kullan](howto-use-dtdl-authoring-tools.md)
