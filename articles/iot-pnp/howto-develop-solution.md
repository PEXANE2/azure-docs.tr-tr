---
title: Azure IoT çözümünden bir IoT Tak ve Çalıştır Önizleme aygıtıyla etkileşimde Microsoft Dokümanlar
description: Bir çözüm geliştiricisi olarak, IoT Tak ve Çalıştır aygıtlarıyla etkileşimde kalmak için SDK hizmetini nasıl kullanacağınız hakkında bilgi edinin.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: how-to
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e349aadfd629202b1c8cdb5c53a88e0a6c2e06de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159226"
---
# <a name="connect-to-and-interact-with-an-iot-plug-and-play-preview-device"></a>Bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanma ve bunlarla etkileşimkurma

Bu nasıl yapılabilir kılavuzu, IoT Çözümünüzün IoT Tak ve Çalıştır Önizleme aygıtlarıyla nasıl etkileşimde olabileceğini gösteren Düğüm hizmeti SDK'daki örnekleri nasıl kullanacağınızı gösterir.

[IoT Tak ve Çalıştır cihazını çözümünüze](quickstart-connect-pnp-device-solution-node.md) hızlı bir şekilde bağlamayı tamamlamadıysanız, bunu hemen yapmalısınız. Hızlı başlangıç, SDK'yı nasıl indirip yükleyip bazı örnekleri çalıştırabileceğinizi gösterir.

Hizmet örneklerini çalıştırmadan önce, yeni bir terminal açın, klonlanmış deponuzun kök klasörüne gidin, **digitaltwins/quickstarts/service** klasörüne gidin ve bağımlılıkları yüklemek için aşağıdaki komutu çalıştırın:

```cmd/sh
npm install
```

## <a name="run-the-service-samples"></a>Hizmet örneklerini çalıştırma

Düğüm.js hizmeti SDK'nın yeteneklerini keşfetmek için aşağıdaki örnekleri kullanın. Ortam değişkeninin `IOTHUB_CONNECTION_STRING` kullandığınız kabukta ayarlandığınızdan emin olun:

### <a name="retrieve-a-digital-twin-and-list-the-interfaces"></a>Dijital ikiz alın ve arayüzleri listele

**get_digital_twin.js,** aygıtınızla ilişkili dijital ikizi alır ve bileşenini komut satırına yazdırır. Başarılı olmak için çalışan bir aygıt örneği gerektirmez.

**get_digital_twin_interface_instance.js,** aygıtınızla ilişkili tek bir dijital ikiz arabirimi örneğini alır ve komut satırına yazdırır. Cihaz örneğinin çalışmasını gerektirmez.

### <a name="get-and-set-properties-using-the-node-service-sdk"></a>Düğüm hizmeti SDK'yı kullanarak özellikleri alın ve ayarlayın

**update_digital_twin.js,** tam bir yama kullanarak cihazınızın dijital ikizinde yazılabilir bir özelliği güncelleştirir. İsterseniz birden çok arabirimde birden çok özelliği güncelleştirebilirsiniz. Başarılı olması için aygıt örneğinin aynı anda çalışıyor olması gerekir. Başarı, aygıt örneğinin, servis örneğinin terminalde güncelleştirilmiş bir dijital ikizi yazdırma özelliğini güncelleştirmeyle ilgili bir şey yazdırıyor gibi görünüyor.

### <a name="send-a-command-and-retrieve-the-response-using-the-node-service-sdk"></a>Bir komut gönderin ve Düğüm hizmeti SDK kullanarak yanıtı alın

**invoke_command.js** cihazınızın dijital ikizinde eşzamanlı bir komut çağırır. Başarılı olması için aygıt örneğinin aynı anda çalışıyor olması gerekir. Başarı, aygıt örneğinin bir komutu kabul etme yle ilgili bir şey yazdırması ve servis istemcisinin terminaldeki komutun sonucunu yazdırması gibi görünüyor.

### <a name="connect-to-the-public-repository-and-retrieve-a-model-definition-using-the-node-service-sdk"></a>Genel depoya bağlanın ve Düğüm hizmeti SDK'yı kullanarak bir model tanımı alın

Hizmet ve aygıt örnekleri yle aynı yönergeleri kullanarak aşağıdaki ortam değişkenini ayarlamanız gerekir:

* `AZURE_IOT_MODEL_REPOSITORY_CONNECTION_STRING`

Bu bağlantı dizesini, **Şirket deponuz**için Bağlantı **dizeleri** sekmesinde [IoT için Azure Sertifikalı portalında](https://preview.catalog.azureiotsolutions.com) bulabilirsiniz.

Bağlantı dizesi aşağıdaki örnek gibi görünür:

```text
HostName={repo host name};RepositoryId={repo ID};SharedAccessKeyName={repo key ID};SharedAccessKey={repo key secret}
```

Bu dört ortam değişkenini ayarladıktan sonra, örneği diğer örnekleri çalıştırdığınız gibi çalıştırın:

```cmd/sh
node model_repo.js
```

Bu **örnek, ModelDiscovery** arabirimini karşıdan yükler ve bu modeli terminalde yazdırır.

### <a name="run-queries-in-iot-hub-based-on-capability-models-and-interfaces"></a>IoT Hub'da sorguları yetenek modellerine ve arabirimlerine göre çalıştırma

IoT Hub sorgu `HAS_INTERFACE` dili `HAS_CAPABILITYMODEL` destekler ve aşağıdaki örneklerde gösterildiği gibi:

```sql
select * from devices where HAS_INTERFACE('id without version', version)
```

```sql
select * from devices where HAS_CAPABILITYMODEL('id without version', version)
```

### <a name="creating-digital-twin-routes"></a>Dijital ikiz rotalar oluşturma

Çözümünüz, dijital ikiz değiştirme olaylarıyla ilgili bildirimler alabilir. Bu bildirimlere abone olmak için, bildirimleri blob depolama, Olay Hub'ları veya Hizmet Veri Merkezi kuyruğu gibi bir uç noktaya göndermek için [IoT Hub yönlendirme özelliğini](../iot-hub/iot-hub-devguide-endpoints.md) kullanın.

Dijital ikiz rota oluşturmak için:

1. Azure portalında IoT Hub kaynağınıza gidin.
1. **İleti yönlendirme'yi**seçin.
1. **Rotalar** sekmesinde **Ekle'yi**seçin.
1. **Ad** alanına bir değer girin ve bir **Bitiş Noktası**seçin. Bir bitiş noktası yapılandırmadıysanız, **bitiş noktası ekle'yi**seçin.
1. Veri **kaynağı** açılır durumda, **Dijital İkiz Değiştirme Olayları'nı**seçin.
1. **Kaydet'i**seçin.

Aşağıdaki JSON, dijital ikiz değiştirme olayının bir örneğini gösterir:

```json
{
  "interfaces": {
    "urn_azureiot_ModelDiscovery_DigitalTwin": {
      "name": "urn_azureiot_ModelDiscovery_DigitalTwin",
      "properties": {
        "modelInformation": {
          "reported": {
            "value": {
              "modelId": "urn:domain:capabilitymodel:TestCapability:1",
              "interfaces": {
                "MyInterfaceFoo": "urn:domain:interfaces:FooInterface:1",
                "urn_azureiot_ModelDiscovery_DigitalTwin": "urn:azureiot:ModelDiscovery:DigitalTwin:1"
              }
            }
          }
        }
      }
    },
    "MyInterfaceFoo": {
      "name": "MyInterfaceFoo",
      "properties": {
        "property_1": { "desired": { "value": "value_1" } },
        "property_2": {
          "desired": { "value": 20 },
          "reported": {
            "value": 10,
            "desiredState": {
              "code": 200,
              "version": 22,
              "subCode": 400,
              "description": ""
            }
          }
        },
        "property_3": { "reported": { "value": "value_3" } }
      }
    }
  },
  "version": 4
}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen servis çözümleri hakkında bilgi edindiğinize göre, önerilen bir sonraki adım [Model keşfi](concepts-model-discovery.md)hakkında bilgi edinmektir.
