---
title: Azure IoT Hub mesajlaşmayı anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub ile cihazdan buluta ve bulut-to-aygıt mesajlaşma. İleti biçimleri ve desteklenen iletişim protokolleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: f56332fa7f53c729ffaa28ea375f043d1b4a3678
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60626256"
---
# <a name="send-device-to-cloud-and-cloud-to-device-messages-with-iot-hub"></a>IoT Hub ile aygıttan buluta ve buluttan cihaza iletiler gönderme

IoT Hub, cihazlarınızla çift yönlü iletişim sağlar. Cihazlarınızdan çözümlerinize geri uçlara iletiler göndererek ve IoT çözümlerinizden komutları aygıtlarınıza göndererek cihazlarınızla iletişim kurmak için IoT Hub mesajlaşmasını kullanın. [IoT Hub ileti biçimi](iot-hub-devguide-messages-construct.md)hakkında daha fazla bilgi edinin.

## <a name="sending-device-to-cloud-messages-to-iot-hub"></a>Aygıttan buluta iletileri IoT Hub'ına gönderme

IoT Hub, aygıtlarınızdan gelen telemetri iletilerini okumak için arka uç hizmetleri tarafından kullanılabilecek yerleşik bir hizmet bitiş noktasına sahiptir. Bu uç nokta [Olay Hub'ları](https://docs.microsoft.com/azure/event-hubs/) ile uyumludur ve bu yerleşik [bitiş noktasından okumak](iot-hub-devguide-messages-read-builtin.md)için standart IoT Hub SDK'larını kullanabilirsiniz.

IoT Hub, [ileti yönlendirmesini](iot-hub-devguide-messages-d2c.md)kullanarak aygıt telemetri verilerini ve olaylarını Azure hizmetlerine göndermek için kullanıcılar tarafından tanımlanabilecek [özel uç noktaları](iot-hub-devguide-endpoints.md#custom-endpoints) da destekler.

## <a name="sending-cloud-to-device-messages-from-iot-hub"></a>IoT Hub'dan buluttan aygıta iletigönderme

Çözüm arka uçtan aygıtlarınıza [buluttan aygıta](iot-hub-devguide-messages-c2d.md) iletiler gönderebilirsiniz.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub mesajlaşma işlevinin temel özellikleri iletilerin güvenilirliği ve dayanıklılığıdır. Bu özellikler, aygıt tarafında aralıklı bağlantıesnekliği sağlar ve bulut tarafında olay işleme de ani yükler. IoT Hub, hem aygıtlardan buluta hem de bulutlardan cihaza ileti için *en az bir kez* teslim garantisi uygular.

## <a name="choosing-the-right-type-of-iot-hub-messaging"></a>Doğru IoT Hub ileti türünü seçme

Cihaz uygulamanızdan zaman serisi telemetrisi ve uyarılar göndermek için cihazdan buluta iletiler ve cihaz uygulamanıza tek yönlü bildirimler için buluttan cihaza iletileri kullanın.

* Aygıttan buluta iletiler, bildirilen özellikler veya dosya yükleme arasında seçim yapmak için [Aygıttan buluta iletişim kılavuzuna](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-d2c-guidance) bakın.

* Buluttan aygıta iletiler, istenen özellikler veya doğrudan yöntemler arasında seçim yapmak için [buluttan aygıta iletişim kılavuzuna](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-c2d-guidance) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub [ileti yönlendirme](iot-hub-devguide-messages-d2c.md)si hakkında bilgi edinin.

* IoT Hub [bulut-cihaz mesajlaşma](iot-hub-devguide-messages-c2d.md)hakkında bilgi edinin.