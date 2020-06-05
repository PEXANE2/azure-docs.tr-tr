---
title: Azure IoT Central cihaz şablonları nelerdir | Microsoft Docs
description: Azure IoT Central cihaz şablonları, uygulamanıza bağlı cihazların davranışını belirtmenizi sağlar.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 2d5009086a24a54c9a2ec4734d3c4dcbebb04475
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84418802"
---
# <a name="what-are-device-templates"></a>Cihaz şablonları nelerdir?

_Bu makale, cihaz geliştiricileri ve çözüm oluşturucuları için geçerlidir._

Azure IoT Central 'deki bir cihaz şablonu, uygulamanıza bağlanan bir cihaz türünün özelliklerini ve davranışlarını tanımlayan bir şema ' dir. Örneğin, cihaz şablonu, bir cihazın gönderdiği Telemetriyi tanımlar, böylece IoT Central doğru birimleri ve veri türlerini kullanan görselleştirmeler oluşturabilir.

Çözüm Oluşturucu bir IoT Central uygulamasına cihaz şablonları ekler. Bir cihaz geliştiricisi, cihaz şablonunda tanımlanan davranışları uygulayan Cihaz kodunu yazar.

Bir cihaz şablonu aşağıdaki bölümleri içerir:

- _Bir cihaz yetenek modeli (DCM)_. Cihaz şablonunun bu bölümü, cihazın uygulamanızla nasıl etkileşime gireceğini tanımlar. Bir cihaz geliştiricisi, DCM 'de tanımlanan davranışları uygular.
- _Bulut özellikleri_. Cihaz şablonunun bu bölümü, çözüm geliştiricisinin depolanacak cihaz meta verilerini belirtmesini sağlar. Bulut özellikleri hiçbir zaman cihazlarla eşitlenmez ve uygulamada mevcut değildir. Bulut özellikleri, bir cihaz geliştiricisinin DCM 'yi uygulamak için yazdığı kodu etkilemez.
- _Özelleştirmeler_. Cihaz şablonunun bu bölümü, çözüm geliştiricisinin DCM içindeki bazı tanımları geçersiz kılmasına izin verir. Çözüm geliştiricisi, uygulamanın bir özelliğin görünen adını değiştirme veya telemetri değerini göstermek için kullanılan renk gibi bir değeri nasıl işleyeceğini iyileştirmek isterse, özelleştirmeler yararlı olur. Özelleştirmeler, bir cihaz geliştiricisinin DCM 'yi uygulamak için yazdığı kodu etkilemez.
- _Görünümler_. Cihaz şablonunun bu bölümü, çözüm geliştiricisinin Cihazdaki verileri görüntülemek ve bir cihazı yönetmek ve denetlemek için form görselleştirmeleri tanımlamasına olanak sağlar. Görünümler DCM, bulut özellikleri ve özelleştirmeleri kullanır. Görünümler, bir cihaz geliştiricisinin DCM 'yi uygulamak için yazdığı kodu etkilemez.

## <a name="device-capability-models"></a>Cihaz yetenek modelleri

Bir DCM, cihazın IoT Central uygulamanızla nasıl etkileşime gireceğini tanımlar. Cihaz geliştiricisi, cihazın IoT Central cihazı izleyip yönetebilmesi için DCM 'de tanımlanan davranışları uyguladığından emin olmalıdır. Bir DCM, bir veya daha fazla _arabirimden_oluşur ve her arabirim _telemetri_ türlerinin, _cihaz özelliklerinin_ve _komutlarının_bir koleksiyonunu tanımlayabilir. Çözüm geliştiricisi, DCM 'yi bir cihaz şablonuna tanımlayan bir JSON dosyasını içeri aktarabilir veya bir DCM oluşturmak veya düzenlemek için IoT Central Web Kullanıcı arabirimini kullanabilirsiniz. Web Kullanıcı arabirimi kullanılarak yapılan bir DCM üzerinde yapılan değişiklikler, [cihaz şablonunun sürümlendirimesini](./howto-version-device-template.md)gerektirir.

Bir çözüm geliştiricisi, DCM içeren bir JSON dosyasını da dışa aktarabilir. Bir cihaz geliştiricisi, cihazın IoT Central uygulamayla nasıl iletişim kuracağını anlamak için bu JSON belgesini kullanabilir.

DCM 'yi tanımlayan JSON dosyası [Digital Ikizi tanım dili (DTDL) v1](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)kullanır. IoT Central, JSON dosyasının ayrı dosyalar yerine satır içi tanımlanmış arabirimlerle birlikte DCM içermesini bekliyor.

Tipik bir IoT cihazı şu şekilde yapılır:

- Özel parçalar, cihazınızın benzersiz hale getirme işlemleri.
- Tüm cihazlarda ortak olan standart parçalar.

Bu bölümler bir DCM içinde _arabirimler_ olarak adlandırılır. Arabirimler, cihazınızın uyguladığı her bir bölümün ayrıntılarını tanımlar. Arabirimler DCMs 'lerde yeniden kullanılabilir.

Aşağıdaki örnek, iki arabirime sahip bir ortam algılayıcısı cihazının cihaz yetenek modelinin ana hattını göstermektedir:

```json
{
  "@id": "urn:contoso:sensor_device:1",
  "@type": "CapabilityModel",
  "displayName": "Environment Sensor Capability Model",
  "implements": [
    {
      "@type": "InterfaceInstance",
      "name": "deviceinfo",
      "schema": {
        "@id": "urn:azureiot:DeviceManagement:DeviceInformation:1",
        "@type": "Interface",
        "displayName": "Device Information",
        "@context": "http://azureiot.com/v1/contexts/IoTModel.json",
        "contents": [
          ...
        ]
      }
    },
    {
      "@type": "InterfaceInstance",
      "name": "sensor",
      "schema": {
        "@id": "urn:contoso:EnvironmentalSensor:1",
        "@type": "Interface",
        "displayName": "Environmental Sensor",
        "@context": "http://azureiot.com/v1/contexts/IoTModel.json",
        "contents": [
          ...
        ]
      }
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Yetenek modelinde bazı gerekli alanlar vardır:

- `@id`: basit bir Tekdüzen Kaynak adı biçimindeki benzersiz bir KIMLIK.
- `@type`: Bu nesnenin yetenek modeli olduğunu bildirir.
- `@context`: yetenek modeli için kullanılan DTDL sürümünü belirtir.
- `implements`: cihazınızın uyguladığı arabirimleri listeler.

Implements bölümündeki arabirimler listesindeki her giriş için şunu vardır:

- `name`: arabirimin programlama adı.
- `schema`: yetenek modelinin uyguladığı arabirim.

Bir arabirimin bazı gerekli alanları vardır:

- `@id`: basit bir Tekdüzen Kaynak adı biçimindeki benzersiz bir KIMLIK.
- `@type`: Bu nesnenin bir arabirim olduğunu bildirir.
- `@context`: arabirim için kullanılan DTDL sürümünü belirtir.
- `contents`: cihazınızı oluşturan özellikleri, telemetri ve komutları listeler.

Özellik modeline, görünen ad ve açıklama gibi daha ayrıntılı bilgi eklemek için kullanabileceğiniz bazı isteğe bağlı alanlar vardır.

### <a name="interface"></a>Arabirim

DTDL, cihazınızın yeteneklerini açıklamanıza olanak sağlar. İlgili yetenekler arabirimler halinde gruplandırılır. Arabirimler, cihazınızın bir parçası olan özellikleri, telemetri ve komutları anlatmaktadır:

- `Properties`. Özellikler, cihazınızın durumunu temsil eden veri alanlarıdır. Bir Coolant göndericisinin yerinde durumu gibi, cihazın dayanıklı durumunu göstermek için özellikleri kullanın. Özellikler Ayrıca cihazın bellenim sürümü gibi temel cihaz özelliklerini de temsil edebilir. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz.
- `Telemetry`. Telemetri alanları sensörlerden ölçümleri temsil eder. Cihazınız her bir algılayıcı ölçümü aldığında, algılayıcı verilerini içeren bir telemetri olayı göndermelidir.
- `Commands`. Komutlar, cihazınızın kullanıcılarının cihazda yürütebilmesi için yöntemleri temsil eder. Örneğin, bir fanı değiştirme veya kapatma için bir Reset komutu veya komutu.

Aşağıdaki örnekte, ortam algılayıcısı arabirim tanımı gösterilmektedir:

```json
{
  "@type": "Property",
  "displayName": "Device State",
  "description": "The state of the device. Two states online/offline are available.",
  "name": "state",
  "schema": "boolean"
},
{
  "@type": "Property",
  "displayName": "Customer Name",
  "description": "The name of the customer currently operating the device.",
  "name": "name",
  "schema": "string",
  "writable": true
},
{
  "@type": [
    "Telemetry",
    "SemanticType/Temperature"
  ],
  "description": "Current temperature on the device",
  "displayName": "Temperature",
  "name": "temp",
  "schema": "double",
  "unit": "Units/Temperature/fahrenheit"
},
{
  "@type": "Command",
  "name": "turnon",
  "comment": "This Commands will turn-on the LED light on the device.",
  "commandType": "synchronous"
},
{
  "@type": "Command",
  "name": "turnoff",
  "comment": "This Commands will turn-off the LED light on the device.",
  "commandType": "synchronous"
}
```

Bu örnekte, iki özellik, bir telemetri türü ve iki komut gösterilmektedir. En az bir alan açıklamasında şunu vardır:

- `@type`özelliğin türünü belirtmek için: `Telemetry` , `Property` , veya `Command` .  Bazı durumlarda tür, değerin nasıl işleneceği hakkında bazı varsayımlar yapmak IoT Central sağlamak için bir anlamsal tür içerir.
- `name`Telemetri değeri için.
- `schema`Telemetri veya özelliğin veri türünü belirtmek için. Bu değer, Double, Integer, Boolean veya String gibi bir temel tür olabilir. Karmaşık nesne türleri, diziler ve eşlemeler de desteklenir.
- `commandType`komutun nasıl işleneceğini belirtmek için.

Görünen ad ve açıklama gibi isteğe bağlı alanlar, arabirime ve yeteneklere daha fazla ayrıntı eklemenizi sağlar.

### <a name="properties"></a>Özellikler

Varsayılan olarak, özellikler salt okunurdur. Salt okuma özellikleri, cihazın IoT Central uygulamanızdaki Özellik değeri güncelleştirmelerini bildirdiği anlamına gelir. IoT Central uygulamanız salt okunurdur bir özelliğin değerini ayarlayamıyorum.

Ayrıca, bir özelliği bir arabirim üzerinde yazılabilir olarak işaretleyebilirsiniz. Bir cihaz, IoT Central uygulamanızdan yazılabilir bir özelliğe güncelleştirme alabilir ve uygulamanıza rapor özellik değeri güncellemeleri verebilir.

Özellik değerlerini ayarlamak için cihazların bağlı olması gerekmez. Güncelleştirilmiş değerler cihaz bir sonraki uygulamaya bağlandıktan sonra aktarılır. Bu davranış hem salt okunurdur hem de yazılabilir özellikler için geçerlidir.

Cihazınızdan telemetri göndermek için özellikleri kullanmayın. Örneğin, gibi bir salt okunur özelliği, `temperatureSetting=80` cihaz sıcaklığının 80 olarak ayarlandığı ve cihazın bu sıcaklığa veya bu sıcaklığın devam etmesi için çalıştığı anlamına gelir.

Yazılabilir özellikler için, cihaz uygulaması, özellik değerini alınıp uygulamadığını göstermek için istenen bir durum durum kodu, sürüm ve açıklama döndürür.

### <a name="telemetry"></a>Telemetri

IoT Central, panolar ve grafiklerde Telemetriyi görüntülemenizi ve eşiklere ulaşıldığında eylemleri tetiklemek için kuralları kullanmanızı sağlar. IoT Central telemetri değerlerinin nasıl görüntüleneceğini anlamak için veri türleri, birimler ve görünen adlar gibi DCM içindeki bilgileri kullanır.

IoT Central verilerini dışa aktarma özelliğini kullanarak depolama veya Event Hubs gibi diğer hedeflere telemetri akışını sağlayabilirsiniz.

### <a name="commands"></a>Komutlar

Komutlar zaman uyumlu ya da zaman uyumsuz. Zaman uyumlu bir komutun varsayılan olarak 30 saniye içinde yürütülmesi gerekir ve komut geldiğinde cihazın bağlanması gerekir. Cihaz zaman yanıt verirse veya cihaz bağlı değilse, komut başarısız olur.

Uzun süre çalışan işlemler için zaman uyumsuz komutları kullanın. Cihaz, telemetri iletilerini kullanarak ilerleme bilgileri gönderir. Bu ilerleme iletileri aşağıdaki üst bilgi özelliklerine sahiptir:

- `iothub-command-name`: Örneğin, komut adı `UpdateFirmware` .
- `iothub-command-request-id`: sunucu tarafında oluşturulan ve ilk çağrıda cihaza gönderilen istek KIMLIĞI.
- `iothub-interface-id`: Bu komutun tanımlanmış olduğu arabirimin KIMLIĞI (örneğin,) `urn:example:AssetTracker:1` .
 `iothub-interface-name`: Örneğin, bu arabirimin örnek adı `myAssetTracker` .
- `iothub-command-statuscode`: Örneğin, cihazdan döndürülen durum kodu `202` .

## <a name="cloud-properties"></a>Bulut özellikleri

Bulut özellikleri, cihaz şablonunun bir parçasıdır, ancak DCM 'nin bir parçası değildir. Bulut özellikleri, çözüm geliştiricisinin IoT Central uygulamasında depolanacak cihaz meta verilerini belirtmesini sağlar. Bulut özellikleri, bir cihaz geliştiricisinin DCM 'yi uygulamak için yazdığı kodu etkilemez.

Bir çözüm geliştiricisi, uygulamaya bağlı cihazları yönetmek için bir operatör etkinleştirmek üzere cihaz özellikleriyle birlikte panolara ve görünümlere bulut özellikleri ekleyebilir. Bir çözüm geliştiricisi, bir eşik değerini bir operatör tarafından düzenlenebilir hale getirmek için bir kural tanımının parçası olarak bulut özelliklerini de kullanabilir.

## <a name="customizations"></a>Özelleştirmeler

Özelleştirmeler cihaz şablonunun bir parçasıdır, ancak DCM 'nin bir parçası değildir. Özelleştirmeler, çözüm geliştiricisinin DCM içindeki tanımları geliştirmesini veya geçersiz kılmasını sağlar. Örneğin, bir çözüm geliştiricisi bir telemetri türü veya özelliği için görünen adı değiştirebilir. Bir çözüm geliştiricisi, bir dize cihaz özelliği için minimum veya maksimum uzunluk gibi doğrulama eklemek için özelleştirmeleri de kullanabilir.

Özelleştirmeler, bir cihaz geliştiricisinin DCM 'yi uygulamak için yazdığı kodu etkileyebilir. Örneğin, bir özelleştirme minimum ve maksimum dize uzunluklarını veya minimum ve maksimum sayı değerlerini telemetri için ayarlayabilir.

## <a name="views"></a>Görünümler

Bir çözüm geliştiricisi, operatörlerin bağlı cihazları izlemesine ve yönetmesine olanak sağlayan görünümler oluşturur. Görünümler cihaz şablonunun bir parçasıdır, bu nedenle bir görünüm belirli bir cihaz türüyle ilişkilendirilir. Bir görünüm şunları içerebilir:

- Telemetriyi çizilecek grafikler.
- Salt okuma cihaz özelliklerini görüntüleyen kutucuklar.
- , İşlecin yazılabilir cihaz özelliklerini düzenlemesini sağlamak için kutucuklar.
- İşlecin bulut özelliklerini düzenlemesini sağlamak için kutucuklar.
- Bir yük bekleyen komutlar da dahil olmak üzere operatör çağrı komutlarına izin veren kutucuklar.
- Etiketleri, resimleri veya markı metnini görüntüleyen kutucuklar.

Bir görünüme ekleyebileceğiniz telemetri, Özellikler ve komutlar, cihaz şablonundaki DCM, bulut özellikleri ve özelleştirmeler tarafından belirlenir.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz geliştiricisi olarak cihaz şablonları hakkında bilgi edindiğinize göre, cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarını IoT Central nasıl güvenlik altına aldığı hakkında daha fazla bilgi edinmek için, önerilen bir sonraki adım [Azure IoT Central 'a](./concepts-get-connected.md) bağlanın.

Çözüm geliştiricisi olarak bir sonraki adım, bir cihaz şablonu oluşturma hakkında daha fazla bilgi edinmek için [Azure IoT Central uygulamanızda yeni bir IoT cihaz türü tanımlama](./howto-set-up-template.md) ' ı okumalıdır.
