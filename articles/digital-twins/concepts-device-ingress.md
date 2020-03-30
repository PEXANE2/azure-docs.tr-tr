---
title: Cihaz bağlantısı ve telemetri girişi - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'teki bir IoT aygıtından nasıl bağlanıp binek ve telemetri göndereceğinizi öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 5c2c519ece9806b92c3e455d5f550bc2abfc9f3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862484"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Cihaz bağlantısı ve telemetri sorunları

Cihazlar ve sensörler tarafından gönderilen telemetri verileri herhangi bir IoT çözeltisinin belkemiğini oluşturur. Bu farklı kaynakların nasıl temsil edilebildiğini ve bir konum bağlamında nasıl yönetileceğimin IoT uygulama geliştirmedeki başlıca endişelerivardır. Azure Digital Twins, cihazları ve sensörleri uzamsal zeka grafiğiyle birleştirerek IoT çözümleri geliştirme işlemini basitleştirir.

Başlamak için uzamsal grafiğin kökünde bir Azure IoT Hub kaynağı oluşturun. IoT Hub kaynağı, kök alanının altındaki tüm aygıtların ileti göndermesine izin verir. IoT Hub oluşturulduktan sonra, Digital Twins örneğinde sensörlü cihazları kaydedin. Aygıtlar, [Azure IoT aygıtı SDK](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks)üzerinden Dijital İkizler hizmetine veri gönderebilir.

Aygıtları gemiye nasıl getireceğinize ilişkin adım adım kılavuz [için, Digital Twins'i dağıtmak ve yapılandırmak için Öğretici'yi](tutorial-facilities-setup.md)okuyun. Bir bakışta, adımlar şunlardır:

- [Azure portalından](https://portal.azure.com)bir Dijital İkizler örneğini dağıtın.
- Grafiğinizde boşluklar oluşturun.
- Bir IoT Hub kaynağı oluşturun ve grafiğinizdeki bir boşluğa atayın.
- Grafiğinizde aygıtlar ve sensörler oluşturun ve bunları önceki adımlarda oluşturulan alanlara atayın.
- Telemetri iletilerini koşullara göre filtrelemek için bir eşleştirici oluşturun.
- Kullanıcı [tanımlı](concepts-user-defined-functions.md)bir işlev oluşturun ve telemetri iletilerinizin özel olarak işlenmesi için grafikteki bir alana atayın.
- Kullanıcı tanımlı işlevin grafik verilerine erişmesine izin vermek için bir rol atayın.
- Dijital İkizler Yönetimi API'lerinden IoT Hub aygıt bağlantı dizesini alın.
- Aygıttaki aygıt bağlantı dizesini Azure IoT aygıtı SDK ile yapılandırın.

Aşağıdaki bölümlerde, Digital Twins Management API'den IoT Hub aygıt bağlantı dizesini nasıl alacağınızı öğrenirsiniz. Sensör tabanlı telemetri göndermek için IoT Hub telemetri ileti biçimini nasıl kullanacağınızı da öğrenirsiniz. Digital Twins, aldığı her bir telemetri parçasının uzamsal grafikteki bir sensörle ilişkilendirilmelerini gerektirir. Bu gereksinim, verilerin uygun uzamsal bağlam içinde işlendiğinden ve yönlendirildir.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Yönetim API'sinden IoT Hub aygıt bağlantı dizesini alma

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

IoT Hub aygıt bağlantı dizesini almak için aygıt API'sinde bir `includes=ConnectionString` parametre ile GET çağrısı yapın. Verilen aygıtı bulmak için aygıt GUID veya donanım kimliğine filtre uygulayın.

```plaintext
YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_GUID?includes=ConnectionString
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_DEVICE_GUID* | Cihaz kimliği |

```plaintext
YOUR_MANAGEMENT_API_URL/devices?HardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=ConnectionString
```

| Parametre değeri | Şununla değiştir |
| --- | --- |
| *YOUR_DEVICE_HARDWARE_ID* | Aygıt donanım kimliği |

Yanıt yükünde, aygıtın **connectionString** özelliğini kopyalayın. Digital Twins'e veri göndermek için Azure IoT aygıtı SDK'yı aradiğinizde kullanırsınız.

## <a name="device-to-cloud-message"></a>Aygıttan buluta ileti

Cihazınızın ileti biçimini ve yükünü çözümünüzün gereksinimlerine uyacak şekilde özelleştirebilirsiniz. [Azure IoT Aygıt İstemci İleti sınıfı, Message(byte[] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___)tarafından desteklenen bir bayt dizisi veya akış halinde seri hale getirilebilen tüm veri sözleşmesini kullanın. İlgili kullanıcı tanımlı bir işlevdeki veri sözleşmesini çözebildiğiniz sürece, ileti seçtiğiniz özel bir ikili biçim olabilir. Aygıttan buluta ileti için tek bir gereksinim vardır. İletinizin işleme motoruna uygun şekilde yönlendirildiklerinden emin olmak için bir dizi özellik koruyun.

### <a name="telemetry-properties"></a>Telemetri özellikleri

 İletinin yük içeriği 256 KB boyutuna kadar rasgele veri **olabilir.** [`Message.Properties`](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet) Türünün özellikleri için beklenen birkaç gereksinim vardır. Tablo, sistem tarafından desteklenen gerekli ve isteğe bağlı özellikleri gösterir.

| Özellik adı | Değer | Gerekli | Açıklama |
|---|---|---|---|
| **DigitalTwins-Telemetri** | 1.0 | Evet | Sisteme bir ileti tanımlayan sabit bir değer. |
| **DigitalTwins-SensorHardwareId** | `string(72)` | Evet | **İletiyi**gönderen sensörün benzersiz bir tanımlayıcısı. Bu değer, sistemin işlemesi için bir nesnenin **HardwareId** özelliğiyle eşleşmelidir. Örneğin, `00FF0643BE88-CO2`. |
| **YaratılışTimeUtc** | `string` | Hayır | Yükün örnekleme süresini tanımlayan [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) biçimlendirilmiş tarih dizesi. Örneğin, `2018-09-20T07:35:00.8587882-07:00`. |
| **Correlationıd** | `string` | Hayır | Sistemdeki olayları izlemek için kullanılan bir UUID. Örneğin, `cec16751-ab27-405d-8fe6-c68e1412ce1f`.

### <a name="send-your-message-to-digital-twins"></a>Mesajınızı Dijital İkizler'e gönderin

İletinizi Digital Twins'e göndermek için DeviceClient [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) veya [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) çağrısını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins veri işleme ve kullanıcı tanımlı işlev özellikleri hakkında bilgi edinmek için [Azure Digital Twins veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md)okuyun.
