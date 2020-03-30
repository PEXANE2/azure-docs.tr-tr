---
title: Sorun Giderme Azure IoT Hub hatası 401003 IoTHubYetkisiz
description: Hata 401003 IoTHubYetkisiz nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: d0a9f643516af5945037acb3dd1da24b06944171
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284414"
---
# <a name="401003-iothubunauthorized"></a>401003 IoTHubUnauthorized

Bu makalede, **401003 IoTHubYetkisiz** hataların nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

### <a name="symptom-1"></a>Belirti 1

Tanılama günlüklerinde, **401003 IoTHubYetkisiz**ile bağlantı kesen bir aygıt deseni görürsünüz , ardından **404104 DeviceConnectionClosedRemotely**, ve kısa bir süre sonra başarıyla bağlanan.

### <a name="symptom-2"></a>Belirti 2

IoT Hub'a yapılan istekler aşağıdaki hata iletilerinden biriyle başarısız oldu:

* Yetkilendirme üstbilgisi eksik
* IotHub\*' ' belirtilen cihazı\*içermez ' '
* Yetkilendirme kuralı\*' ' '\*için erişime izin vermez ' '
* Bu aygıt için kimlik doğrulama başarısız oldu, belirteç veya sertifikayı yenileyin ve yeniden bağlanma
* Parmak izi yapılandırmaile eşleşmiyor: Thumbprint:\*SHA1Hash=\*, SHA2Hash= ; Yapılandırma: PrimaryThumbprint=\*, İkincilThumbprint=\*

## <a name="cause"></a>Nedeni

### <a name="cause-1"></a>Neden 1

MQTT için, bazı SDK'lar, SAS belirteci ne zaman yenileyeceğimi bilmek için süresi dolduğunda bağlantıyı kesmeyi sağlamak için IoT Hub'ına güvenir. Bu yüzden, 

1. SAS belirteci sona eriyor
1. IoT Hub son kullanma tarihini bildirir ve aygıtı **401003 IoTHubYetkisiz** ile keser
1. Cihaz **404104 DeviceConnectionIle** bağlantıyı kapatırKapalı Uzaktan
1. IoT SDK yeni bir SAS belirteci üretir
1. Aygıt IoT Hub ile başarıyla yeniden bağlanır

### <a name="cause-2"></a>Neden 2

IoT Hub auth üstbilgisini, kuralı veya anahtarı doğrulayamadı.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Aygıt bağlantı dizesini kullanarak bağlantı için IoT SDK kullanıyorsanız hiçbir işlem gerekmez. IoT SDK, SAS belirteci sona erme süresi dolmadan yeniden bağlanmak için yeni belirteci yeniler. 

Hata hacmi bir sorunsa, son kullanma tarihinden önce SAS belirteci yenileyen C SDK'ya geçin. Ayrıca, AMQP için SAS belirteci kopmadan yenilenebilir.

### <a name="solution-2"></a>Çözüm 2

Genel olarak, sunulan hata iletisi hatanın nasıl düzeltilenin isini açıklamalıdır. Herhangi bir nedenle hata iletisi ayrıntısına erişiminiz yoksa, aşağıdakilerden emin olun:

- Kullandığınız SAS veya diğer güvenlik belirteci süresi dolmamış. 
- Yetkilendirme kimlik bilgisi, kullandığınız protokol için iyi oluşturulmuştur. Daha fazla bilgi için [IoT Hub erişim denetimine](iot-hub-devguide-security.md)bakın.
- Kullanılan yetkilendirme kuralı, istenen işlem için izine sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'a kimlik doğrulamayı kolaylaştırmak için [Azure IoT SDK'larını](iot-hub-devguide-sdks.md)kullanmanızı öneririz.