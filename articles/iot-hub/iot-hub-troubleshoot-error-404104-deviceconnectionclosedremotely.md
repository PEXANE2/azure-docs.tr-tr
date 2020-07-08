---
title: Azure IoT Hub sorun giderme hatası 404104 Deviceconnectioncloseduzaktan
description: 404104 Deviceconnectioncloseduzaktan hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: mqtt
ms.openlocfilehash: c8cb91aa0c7ce1610320d4107db282d3c34407ba
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81758732"
---
# <a name="404104-deviceconnectionclosedremotely"></a>404104 DeviceConnectionClosedRemotely

Bu makalede, **404104 Deviceconnectioncloseduzaktan** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

### <a name="symptom-1"></a>Belirti 1

Cihazların düzenli aralıklarla bağlantısını keser (örneğin, her 65 dakikada bir) ve IoT Hub tanılama günlüklerinde **404104 Deviceconnectioncloseduzaktan** ' yi görürsünüz. Bazen **401003 Iothubyetkilendirilmemiş** ve başarılı bir cihaz bağlantısı olayının bir dakikadan daha kısa bir sürede olduğunu da görürsünüz.

### <a name="symptom-2"></a>Belirti 2

Cihazların bağlantısı rastgele kesilir ve IoT Hub tanılama günlüklerinde **404104 Deviceconnectioncloseduzaktan** bakın.

### <a name="symptom-3"></a>Belirti 3

Birçok cihaz bağlantısı kesildiğinde, [bağlı cihazlar ölçümünde](iot-hub-metrics.md)bir DIP görürsünüz ve tanılama günlüklerinde normalden fazla **404104 Deviceconnectioncloseduzaktan** ve [500 xxx iç hata](iot-hub-troubleshoot-error-500xxx-internal-errors.md) vardır.

## <a name="causes"></a>Nedenler

### <a name="cause-1"></a>Neden 1

[IoT Hub bağlanmak için kullanılan SAS belirteci](iot-hub-devguide-security.md#security-tokens) , IoT Hub cihazın bağlantısının kesilip kesilmesine neden olur. Belirteç cihaz tarafından yenilendiğinde bağlantı yeniden oluşturulur. Örneğin, [SAS belirtecinin her saat, her saat için varsayılan olarak zaman aşımına uğrar](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-authentication)ve bu da normal bağlantı kesilmesine yol açabilir.

Daha fazla bilgi için bkz. [401003 Iothubyetkisiz nedeni](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#cause-1).

### <a name="cause-2"></a>Neden 2

Bazı olanaklar şunlardır:

- Cihazın arka plandaki ağ bağlantısı, bir uzak boşta kalma zaman aşımı [ile sonuçlanan MQTT etkin](iot-hub-mqtt-support.md#default-keep-alive-timeout)'den uzun MQTT Keep-canlı ayarı cihaz başına farklı olabilir.

- Cihaz TCP/IP düzeyinde sıfırlama gönderdi, ancak uygulama düzeyi göndermedi `MQTT DISCONNECT` . Temel olarak, cihaz temeldeki yuva bağlantısını aniden kapattı. Bazen bu sorun, Azure IoT SDK 'sının eski sürümlerindeki hataların oluşmasına neden olur.

- Cihaz tarafı uygulaması kilitlendi.

### <a name="cause-3"></a>Neden 3

IoT Hub geçici bir sorun yaşıyor olabilir. [IoT Hub iç sunucu hatası nedeni](iot-hub-troubleshoot-error-500xxx-internal-errors.md#cause)' ne bakın.

## <a name="solutions"></a>Çözümler

### <a name="solution-1"></a>Çözüm 1

Bkz. [401003 Iothubyetkilendirilmemiş çözüm 1](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#solution-1)

### <a name="solution-2"></a>Çözüm 2

- Cihazın [bağlantıyı test](tutorial-connectivity.md)ederek IoT Hub iyi bir bağlantı olduğundan emin olun. Ağ güvenilmez veya kesintili ise, algılama işleminin (örneğin, Azure Izleyici uyarıları aracılığıyla) daha uzun sürmesiyle sonuçlanabileceğinden, etkin tut değerinin arttırılmayı önermiyoruz. 

- [IoT SDK](iot-hub-devguide-sdks.md)'larının en son sürümlerini kullanın.

### <a name="solution-3"></a>Çözüm 3

[IoT Hub iç sunucu hatalarını giderme çözümlerine](iot-hub-troubleshoot-error-500xxx-internal-errors.md#solution)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantıları güvenilir bir şekilde yönetmek için Azure IoT cihaz SDK 'larını kullanmanızı öneririz. Daha fazla bilgi edinmek için bkz. [Azure IoT Hub cihaz SDK 'larını kullanarak bağlantı ve güvenilir mesajlaşmayı yönetme](iot-hub-reliability-features-in-sdks.md)