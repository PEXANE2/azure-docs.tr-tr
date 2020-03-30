---
title: Geliştirici kılavuzu - IoT Tak ve Çalıştır Önizleme | Microsoft Dokümanlar
description: IoT Tak ve Çalıştır geliştiricileri için cihaz modelleme açıklaması
author: dominicbetts
ms.author: dobett
ms.date: 12/26/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 5fda51e6d2f62b9cbef0fcac22d5bb2ea0df905b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605210"
---
# <a name="iot-plug-and-play-preview-modeling-developer-guide"></a>IoT Tak ve Çalıştır Önizleme modelleme geliştirici kılavuzu

IoT Tak ve Çalıştır Önizleme, yeteneklerini Azure IoT uygulamalarına tanıtan aygıtlar oluşturmanıza olanak tanır. IoT Tak ve Çalıştır aygıtları, bir müşteri bunları IoT Tak ve Çalıştır özellikli uygulamalara bağladığında manuel yapılandırma gerektirmez. IoT Central, IoT Tak ve Çalıştır özellikli bir uygulama örneğidir.

Bir IoT Tak ve Çalıştır aygıtı oluşturmak için bir aygıt açıklaması oluşturmanız gerekir. Açıklama Dijital İkizler Tanım Dili (DTDL) adı verilen basit bir tanım dili ile yapılır.

## <a name="device-capability-model"></a>Cihaz yetenek modeli

DTDL ile, cihazınızın parçalarını açıklamak için bir _aygıt yeteneği modeli_ oluşturursunuz. Tipik bir IoT aygıtı oluşur:

- Cihazınızı benzersiz yapan özel parçalar.
- Standart parçalar, tüm cihazlariçin ortak olan şeylerdir.

Bu parçalara aygıt yeteneği modelinde _arabirim_ denir. Arabirimler, cihazınızın uyguladığı her bölümün ayrıntılarını tanımlar.

Aşağıdaki örnekte, bir termostat aygıtının cihaz yetenek modeli gösterilmektedir:

```json
{
  "@id": "urn:example:Thermostat_T_1000:1",
  "@type": "CapabilityModel",
  "implements": [
    {
      "name": "thermostat",
      "schema": "urn:example:Thermostat:1"
    },
    {
      "name": "urn_azureiot_deviceManagement_DeviceInformation",
      "schema": "urn:azureiot:deviceManagement:DeviceInformation:1"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Yetenek modelinin bazı gerekli alanları vardır:

- `@id`: basit bir Tekdüzen Kaynak Adı şeklinde benzersiz bir kimlik.
- `@type`: bu nesnenin bir yetenek modeli olduğunu bildirir.
- `@context`: yetenek modeli için kullanılan DTDL sürümünü belirtir.
- `implements`: cihazınızın uyguladığı arabirimleri listeler.

Uygulamalar bölümündeki arabirimler listesindeki her girişin aşağıdakileri vardır:

- `name`: arabirimin programlama adı.
- `schema`: yetenek modelinin uyguladığı arayüz.

Görüntü adı ve açıklama gibi yetenek modeline daha fazla ayrıntı eklemek için kullanabileceğiniz isteğe bağlı ek alanlar vardır. Bir yetenek modeli içinde bildirilen arabirimler aygıtın bileşenleri olarak düşünülebilir. Genel önizleme için arabirim listesinin şema başına yalnızca bir girişi olabilir.

## <a name="interface"></a>Arabirim

DTDL ile, arabirimleri kullanarak cihazınızın özelliklerini açıklarsınız. Arabirimler _özellikleri_açıklar , _telemetri_, ve _cihazınızın_ bir bölümünü komutları uygular:

- `Properties`. Özellikler, cihazınızın durumunu temsil eden veri alanlarıdır. Soğutma sıvısı pompasının bekleme durumu gibi aygıtın dayanıklı durumunu temsil eden özellikleri kullanın. Özellikler, aygıtın firmware sürümü gibi temel aygıt özelliklerini de temsil edebilir. Özellikleri salt okunur veya yazılabilir olarak bildirebilirsiniz.
- `Telemetry`. Telemetri alanları sensörlerden gelen ölçümleri temsil eder. Cihazınız bir sensör ölçümü aldığında, sensör verilerini içeren bir telemetri olayı göndermelidir.
- `Commands`. Komutlar, cihazınızın kullanıcılarının aygıtta çalıştırabileceği yöntemleri temsil eder. Örneğin, bir fanı açıp kapatmak için bir sıfırlama komutu veya komut.

Aşağıdaki örnekte bir termostat aygıtının arabirimi gösterilmektedir:

```json
{
  "@id": "urn:example:Thermostat:1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Telemetry",
      "name": "temperature",
      "schema": "double"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Arabirimin bazı gerekli alanları vardır:

- `@id`: basit bir Tekdüzen Kaynak Adı şeklinde benzersiz bir kimlik.
- `@type`: bu nesnenin bir arabirim olduğunu bildirir.
- `@context`: arayüz için kullanılan DTDL sürümünü belirtir.
- `contents`: cihazınızı oluşturan özellikleri, telemetriyi ve komutları listeler.

Bu basit örnekte, sadece tek bir telemetri alanı vardır. En az alan açıklaması:

- `@type`: yeteneğin türünü belirtir: `Telemetry` `Property`, `Command`, veya .
- `name`: telemetri değerinin adını sağlar.
- `schema`: telemetri için veri türünü belirtir. Bu değer, çift, tamsayı, boolean veya dize gibi ilkel bir tür olabilir. Karmaşık nesne türleri, diziler ve haritalar da desteklenir.

Görüntü adı ve açıklama gibi diğer isteğe bağlı alanlar, arabirime ve yeteneklere daha fazla ayrıntı eklemenize olanak tanır.

### <a name="properties"></a>Özellikler

Varsayılan olarak, özellikler salt okunur. Salt okunur özellikler, aygıtın Özellik değeri güncelleştirmelerini IoT hub'ınıza bildirdiği anlamına gelir. IoT hub'ınız salt okunur özelliğin değerini ayarlayamaz.

Ayrıca, bir özelliği arabirimde yazılabilir olarak işaretleyebilirsiniz. Aygıt, IoT hub'ınızdan yazılabilir bir özellik için güncelleştirme alabilir ve hub'ınıza özellik değeri güncelleştirmelerini bildirebilir.

Aygıtların özellik değerlerini ayarlamak için bağlanması gerekmemektedir. Aygıt hub'a bağlandığında güncelleştirilen değerler aktarılır. Bu davranış, hem salt okunur hem de yazılabilir özellikler için geçerlidir.

Cihazınızdan telemetri göndermek için özellikleri kullanmayın. Örneğin, `temperatureSetting=80` aygıt sıcaklığının 80 olarak ayarlandığı ve aygıtın bu sıcaklığa ulaşmaya veya bu sıcaklıkta kalmaya çalıştığı anlamına gelir.

Yazılabilir özellikler için aygıt uygulaması, özellik değerini alıp almadığını ve uygulanıp uygulanmadığını belirtmek için istenen durum durumu kodunu, sürümünü ve açıklamasını döndürür.

### <a name="telemetry"></a>Telemetri

Varsayılan olarak, IoT Hub aygıtlardan gelen tüm telemetri iletilerini [Olay Hub'larıyla](https://azure.microsoft.com/documentation/services/event-hubs/)uyumlu [yerleşik hizmete bakan bitiş noktasına **(iletiler/olaylar)**](../iot-hub/iot-hub-devguide-messages-read-builtin.md) yönlendirir.

[IoT Hub'ın özel uç noktalarını ve yönlendirme kurallarını](../iot-hub/iot-hub-devguide-messages-d2c.md) kullanarak blob depolama veya diğer olay hub'ları gibi diğer hedeflere telemetri gönderebilirsiniz. Yönlendirme kuralları iletileri seçmek için ileti özelliklerini kullanır.

### <a name="commands"></a>Komutlar

Komutlar ya senkron ya da eşzamanlıdır. Senkron komut varsayılan olarak 30 saniye içinde yürütülmelidir ve komut geldiğinde aygıt bağlanmalıdır. Aygıt zamanında yanıt verirse veya aygıt bağlı değilse, komut başarısız olur.

Uzun süren işlemler için eşzamanlı komutları kullanın. Cihaz, telemetri iletilerini kullanarak ilerleme bilgileri gönderir. Bu ilerleme iletileri aşağıdaki üstbilgi özelliklerine sahiptir:

- `iothub-command-name`: komut adı, `UpdateFirmware`örneğin .
- `iothub-command-request-id`: sunucu tarafında oluşturulan ve ilk çağrıda cihaza gönderilen istek kimliği.
- `iothub-interface-id`: Bu komutun tanımlandığı arabirimin kimliği, örneğin. `urn:example:AssetTracker:1`
 `iothub-interface-name`: örneğin, bu arabirimin `myAssetTracker`örnek adı .
- `iothub-command-statuscode`: örneğin `202`cihazdan döndürülen durum kodu .

## <a name="register-a-device"></a>Cihaz kaydetme

IoT Tak ve Çalıştır, cihazınızın yeteneklerinin reklamını yapmayı kolaylaştırır. IoT Tak ve Çalıştır ile cihazınız IoT Hub'a bağlandıktan sonra cihaz yetenek modelinizi kaydetmeniz gerekir. Kayıt, müşterilerin cihazınızın IoT Tak ve Çalıştır özelliklerini kullanmasını sağlar.

Bu kılavuz, C için Azure IoT Aygıt SDK'sını kullanarak bir aygıtı nasıl kaydedebilirsiniz gösterir.

Cihazınızın uyguladığı her arabirim için bir arabirim oluşturmanız ve bu arabirimi uygulamasına bağlamanız gerekir.

C SDK'yı kullanarak daha önce gösterilen termostat arabirimi için, termostat arabiriminizi oluşturur ve uygulamasına bağlarsınız:

```c
DIGITALTWIN_INTERFACE_HANDLE thermostatInterfaceHandle;

DIGITALTWIN_CLIENT_RESULT result = DigitalTwin_InterfaceClient_Create(
    "thermostat",
    "urn:example:Thermostat:1",
    null, null,
    &thermostatInterfaceHandle);

result = DigitalTwin_Interface_SetCommandsCallbacks(
    thermostatInterfaceHandle,
    commandsCallbackTable);

result = DigitalTwin_Interface_SetPropertiesUpdatedCallbacks(
    thermostatInterfaceHandle,
    propertiesCallbackTable);

```

Cihazınızın uyguladığı her arabirim için bu kodu yineleyin.

Bir arayüz oluşturduktan sonra, aygıt yetenek modelinizi ve arabirimlerinizi IoT hub'ınızla kaydedin:

```c
DIGITALTWIN_INTERFACE_CLIENT_HANDLE interfaces[2];
interfaces[0] = thermostatInterfaceHandle;
interfaces[1] = deviceInfoInterfaceHandle;

result = DigitalTwin_DeviceClient_RegisterInterfacesAsync(
    digitalTwinClientHandle, // The handle for the connection to Azure IoT
    "urn:example:Thermostat_T_1000:1",
    interfaces, 2,
    null, null);
```

## <a name="use-a-device"></a>Cihaz kullanma

IoT Tak ve Çalıştır, yeteneklerini IoT hub'ınızla kaydetmiş cihazları kullanmanıza olanak tanır. Örneğin, aygıtın özelliklerine ve komutlarına doğrudan erişebilirsiniz.

IoT hub'ınıza bağlı bir IoT Tak ve Çalıştır aygıtı kullanmak için IoT Hub REST API veya IoT dil SDK'larından birini kullanın. Aşağıdaki örneklerde IoT Hub REST API'yi kullanılır. API'nin geçerli sürümü `2019-07-01-preview`. REST `?api-version=2019-07-01-preview` PI aramalarınıza ek olarak.

Termostattaki`fwVersion` `DeviceInformation` arabirimdeki firmware sürümü gibi bir aygıt özelliğinin değerini almak için dijital ikizler REST API'yi kullanırsınız.

Termostat aygıtınız `t-123`adı ve ise, cihazınız tarafından uygulanan tüm arabirimlerdeki tüm özellikleri REST API GET çağrısı ile alırsınız:

```REST
GET /digitalTwins/t-123/interfaces
```

Daha genel olarak, tüm arabirimlerdeki tüm özelliklere, aygıtın `{device-id}` tanımlayıcısı olan bu REST API şablonuyla erişilir:

```REST
GET /digitalTwins/{device-id}/interfaces
```

Arabirimin adını biliyorsanız ve söz `deviceInformation`konusu arabirim için özellikler almak istiyorsanız, isteği ada göre belirli bir arabirime göre kapsama:

```REST
GET /digitalTwins/t-123/interfaces/deviceInformation
```

Daha genel olarak, belirli bir arabirimin özelliklerine aygıtın tanımlayıcısı olan ve `device-id` `{interface-name}` arabirimin adı olan bu REST API şablonu aracılığıyla erişilebilir:

```REST
GET /digitalTwins/{device-id}/interfaces/{interface-name}
```

IoT Tak ve Çalıştır aygıt komutlarını doğrudan arayabilirsiniz. Aygıttaki `Thermostat` arabirimde bir `restart` komut varsa, rest API POST çağrısı yla arayabilirsiniz: `t-123`

```REST
POST /digitalTwins/t-123/interfaces/thermostat/commands/restart
```

Daha genel olarak, komutlar bu REST API şablonu aracılığıyla çağrılabilir:

- `device-id`: aygıtın tanımlayıcısı.
- `interface-name`: aygıt yetenek modelindeki uygulamalar bölümünden arabirimin adı.
- `command-name`: emrin adı.

```REST
/digitalTwins/{device-id}/interfaces/{interface-name}/commands/{command-name}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık aygıt modellemesi hakkında bilgi edindiğinize göre, bazı ek kaynaklar şunlardır:

- [Dijital İkiz Tanımlı Dil (DTDL)](https://aka.ms/DTDL)
- [C cihaz API’si](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](https://docs.microsoft.com/rest/api/iothub/device)
