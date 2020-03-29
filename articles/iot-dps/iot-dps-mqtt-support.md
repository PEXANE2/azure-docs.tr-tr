---
title: Azure IoT Cihaz Sağlama Hizmeti MQTT desteğini anlayın | Microsoft Dokümanlar
description: Geliştirici kılavuzu - MQTT protokolünü kullanarak Azure IoT Aygıt Sağlama Hizmeti'ne (DPS) aygıta bakan bitiş noktasına bağlanan aygıtlar için destek.
author: rajeevmv
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: ravokkar
ms.openlocfilehash: ea6ece7e34ddb9c25f9f8349239ab3a1c3405abf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973382"
---
# <a name="communicate-with-your-dps-using-the-mqtt-protocol"></a>MQTT protokolünü kullanarak DPS'nizle iletişim kurun

DPS, aygıtların DPS aygıt bitiş noktası ile aşağıdakileri kullanarak iletişim kurmasını sağlar:

* [MQTT v3.1.1](https://mqtt.org/) bağlantı noktası 8883
* [MQTT v3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718127) bağlantı noktası 443 websocket üzerinde.

DPS tam özellikli bir MQTT aracısı değildir ve MQTT v3.1.1 standardında belirtilen tüm davranışları desteklemez. Bu makalede, aygıtların DPS ile iletişim kurmak için desteklenen MQTT davranışlarını nasıl kullanabileceği açıklanmaktadır.

DPS ile tüm cihaz iletişimi TLS/SSL kullanılarak güvence altına alınmalıdır. Bu nedenle, DPS bağlantı noktası 1883 üzerinden güvenli olmayan bağlantıları desteklemez.

 > [!NOTE] 
 > DPS şu anda MQTT protokolü üzerinden TPM [attestation mekanizması](https://docs.microsoft.com/azure/iot-dps/concepts-device#attestation-mechanism) nı kullanan aygıtları desteklememektedir.

## <a name="connecting-to-dps"></a>DPS'ye bağlanma

Aygıt, aşağıdaki seçeneklerden herhangi birini kullanarak DPS'ye bağlanmak için MQTT protokolünü kullanabilir.

* [Azure IoT Sağlama SDK'larında kitaplıklar.](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#microsoft-azure-provisioning-sdks)
* MQTT protokolü doğrudan.

## <a name="using-the-mqtt-protocol-directly-as-a-device"></a>MQTT protokolünü doğrudan kullanma (aygıt olarak)

Bir aygıt Aygıtı SDK'ları kullanamıyorsa, bağlantı noktası 8883'teki MQTT protokolünü kullanarak ortak aygıt uç noktalarına bağlanmaya devam edebilir. **CONNECT** paketinde aygıt aşağıdaki değerleri kullanmalıdır:

* **ClientId** alanı için **registrationId'i**kullanın.

* Kullanıcı **adı** alanı için `{idScope}/registrations/{registration_id}/api-version=2019-03-31`DPS'nin `{idScope}` [idScope'u](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope) nerede kullanılır.

* **Parola** alanı için bir SAS belirteci kullanın. SAS belirteci biçimi hem HTTPS hem de AMQP protokolleri için aynıdır:

  `SharedAccessSignature sr={URL-encoded-resourceURI}&sig={signature-string}&se={expiry}&skn=registration`ResourceURI biçiminde `{idScope}/registrations/{registration_id}`olmalıdır. İlke adı. `registration`

  > [!NOTE]
  > X.509 sertifika kimlik doğrulaması kullanıyorsanız, SAS belirteç parolaları gerekmez.

  SAS belirteçlerinin nasıl üretilenhakkında daha fazla bilgi için, [DPS'ye Erişim denetiminin](how-to-control-access.md#security-tokens)güvenlik belirteçleri bölümüne bakın.

Aşağıda DPS uygulamasına özgü davranışların bir listesi vetir:

 * DPS, **CleanSession** bayrağının **0**olarak ayarlandığı işlevselliğini desteklemez.

 * Bir cihaz uygulaması **QoS 2**ile bir konuya abone olduğunda, DPS **SUBACK** paketinde maksimum QoS düzeyi 1 verir. Bundan sonra DPS, QoS 1 kullanarak cihaza iletiler iletilir.

## <a name="tlsssl-configuration"></a>TLS/SSL yapılandırması

MQTT protokolünü doğrudan kullanmak için müşterinizin TLS 1.2 üzerinden bağlanması *gerekir.* Bu adımı atlama girişimleri bağlantı hataları ile başarısız oldu.


## <a name="registering-a-device"></a>Aygıt kaydetme

Bir aygıtı DPS üzerinden kaydetmek için, `$dps/registrations/res/#` bir aygıtın **Konu Filtresi**olarak abone olması gerekir. Konu Filtresi'ndeki `#` çok düzeyli joker karakter yalnızca aygıtın konu adında ek özellikler almasına izin vermek için kullanılır. DPS alt konuların filtrelemesi için joker kartların `#` veya `?` joker kartların kullanımına izin vermez. DPS genel amaçlı bir pub-alt mesajlaşma aracısı olmadığından, yalnızca belgelenen konu adlarını ve konu filtrelerini destekler.

Cihaz, Konu `$dps/registrations/PUT/iotdps-register/?$rid={request_id}` **Adı**olarak dps'ye bir kayıt iletisi yayımlamalıdır. Yük, JSON formatında [Aygıt Kaydı](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice#deviceregistration) nesnesini içermelidir.
Başarılı bir senaryoda, aygıt x'in `$dps/registrations/res/202/?$rid={request_id}&retry-after=x` saniyeler içinde yeniden deneme değeri olduğu konu adı hakkında bir yanıt alır. Yanıtın yükü JSON biçiminde [RegistrationOperationStatus](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice#registrationoperationstatus) nesnesini içerir.

## <a name="polling-for-registration-operation-status"></a>Kayıt işlem durumu için yoklama

Cihaz kayıt işleminin sonucunu almak için hizmeti düzenli aralıklarla yoklaması gerekir. Aygıtın yukarıda belirtildiği gibi `$dps/registrations/res/#` konuya zaten abone olduğunu varsayarsak, konu adına `$dps/registrations/GET/iotdps-get-operationstatus/?$rid={request_id}&operationId={operationId}` bir işlem durumu iletisi yayımlayabilir. Bu iletideki işlem kimliği, önceki adımdaki Kayıt İşlem Durumu yanıt iletisinde alınan değer olmalıdır. Başarılı durumda, hizmet `$dps/registrations/res/200/?$rid={request_id}` konuyla ilgili olarak yanıt verecektir. Yanıtın yükü RegistrationOperationStatus nesnesini içerir. Yanıt kodu, yeniden deneme densonraki döneme eşit bir gecikmeden sonra 202 ise, aygıt hizmeti yoklamaya devam etmelidir. Hizmet 200 durum kodu döndürürse aygıt kayıt işlemi başarılı olur.

## <a name="connecting-over-websocket"></a>Websocket üzerinden bağlanma
Websocket üzerinden bağlanırken, alt protokolü `mqtt`' ni . [RFC 6455](https://tools.ietf.org/html/rfc6455)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

MQTT protokolü hakkında daha fazla bilgi edinmek için [MQTT belgelerine](https://mqtt.org/documentation)bakın.

DPS'nin yeteneklerini daha fazla keşfetmek için bkz:

* [IoT DPS Hakkında](about-iot-dps.md)
