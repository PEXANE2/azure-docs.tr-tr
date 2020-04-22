---
title: Sorun Giderme Azure IoT Hub hatası 404104 DeviceConnectionClosedRemotely
description: Hata 404104 DeviceConnectionClosedRemotely nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: mqtt
ms.openlocfilehash: c8cb91aa0c7ce1610320d4107db282d3c34407ba
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758732"
---
# <a name="404104-deviceconnectionclosedremotely"></a>404104 DeviceConnectionClosedRemotely

Bu **makalede, 404104 DeviceConnectionClosedRemotely** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

### <a name="symptom-1"></a>Belirti 1

Aygıtlar düzenli aralıklarla (örneğin her 65 dakikada bir) bağlantısını keser ve IoT Hub tanı günlüklerinde **404104 DeviceConnectionClosedRemotely'yi** görürsünüz. Bazen, **401003 IoTHubYetkisiz** ve başarılı bir aygıt bağlantısı olayı bir dakikadan kısa bir süre sonra da görürsünüz.

### <a name="symptom-2"></a>Belirti 2

Aygıtlar bağlantıyı rasgele keser ve IoT Hub tanı günlüklerinde **404104 DeviceConnectionClosedRemotely'yi** görürsünüz.

### <a name="symptom-3"></a>Belirti 3

Birçok aygıt aynı anda bağlantısını keser, [bağlı aygıtlar metrik](iot-hub-metrics.md)bir dip görmek ve daha **fazla 404104 DeviceConnectionClosedRemotely** ve [500xxx Dahili hatalar](iot-hub-troubleshoot-error-500xxx-internal-errors.md) tanı günlükleri normalden daha vardır.

## <a name="causes"></a>Nedenler

### <a name="cause-1"></a>Neden 1

[IoT Hub'a bağlanmak için kullanılan SAS belirteci](iot-hub-devguide-security.md#security-tokens) süresi doldu ve bu da IoT Hub'ın aygıtın bağlantısını kesmesine neden oldu. Bağlantı, belirteç aygıt tarafından yenilendiğinde yeniden kurulur. Örneğin, [SAS belirteci](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-authentication)c SDK için varsayılan olarak her saat sona erer , hangi düzenli keser neden olabilir.

Daha fazla bilgi için bkz: [401003 IoTHubYetkisiz neden.](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#cause-1)

### <a name="cause-2"></a>Neden 2

Bazı olasılıklar şunlardır:

- Cihaz, [MQTT'nin canlı tutulmasından](iot-hub-mqtt-support.md#default-keep-alive-timeout)daha uzun süre altta yatan ağ bağlantısını kaybederek uzaktan boşta zaman aşımına neden olur. MQTT canlı tutma ayarı cihaz başına farklı olabilir.

- Aygıt Bir TCP/IP düzeyi sıfırlama gönderdi, ancak uygulama düzeyi `MQTT DISCONNECT`göndermedi. Temel olarak, cihaz aniden altta yatan soket bağlantısı kapattı. Bazen bu sorun, Azure IoT SDK'nın eski sürümlerindeki hatalardan kaynaklanır.

- Aygıt tarafı uygulaması çöktü.

### <a name="cause-3"></a>Neden 3

IoT Hub geçici bir sorun yaşıyor olabilir. [Bkz. IoT Hub iç sunucu hatası nedeni.](iot-hub-troubleshoot-error-500xxx-internal-errors.md#cause)

## <a name="solutions"></a>Çözümler

### <a name="solution-1"></a>Çözüm 1

Bkz. [401003 IoTHubYetkisiz çözüm 1](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#solution-1)

### <a name="solution-2"></a>Çözüm 2

- Bağlantıyı test ederek aygıtın IoT Hub'a iyi bir [bağlantıya](tutorial-connectivity.md)sahip olduğundan emin olun. Ağ güvenilir değilse veya aralıklıysa, algılamaya (örneğin Azure Monitor uyarıları aracılığıyla) daha uzun süre neden olabileceğinden canlı kalma değerini artırmanızı önermiyoruz. 

- [IoT SDK'ların](iot-hub-devguide-sdks.md)en son sürümlerini kullanın.

### <a name="solution-3"></a>Çözüm 3

[IoT Hub dahili sunucu hatalarının çözümlerine](iot-hub-troubleshoot-error-500xxx-internal-errors.md#solution)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantıları güvenilir bir şekilde yönetmek için Azure IoT aygıt SDK'larını kullanmanızı öneririz. Daha fazla bilgi için [Azure IoT Hub aygıt SDK'larını kullanarak bağlantıyı ve güvenilir mesajlaşmayı yönet'e](iot-hub-reliability-features-in-sdks.md) bakın