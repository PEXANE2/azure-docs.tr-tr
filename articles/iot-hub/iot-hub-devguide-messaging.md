---
title: Azure IoT Hub iletilerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu-IoT Hub ile cihazdan buluta ve buluttan cihaza mesajlaşma. İleti biçimleri ve desteklenen iletişim protokolleri hakkında bilgiler içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- 'Role: Cloud Development'
ms.openlocfilehash: e25b9d6cbe8446fb2f10150f17f7e2b0ccf85d3c
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147625"
---
# <a name="send-device-to-cloud-and-cloud-to-device-messages-with-iot-hub"></a>IoT Hub ile cihazdan buluta ve buluttan cihaza iletileri gönderme

IoT Hub cihazlarınızla çift yönlü iletişim sağlar. Cihazlarınızdan gelen iletileri arka uca göndererek ve IoT çözümlerinizin arka ucundan cihazlarınıza komut göndererek cihazlarınızla iletişim kurmak için IoT Hub mesajlaşma kullanın. [IoT Hub ileti biçimi](iot-hub-devguide-messages-construct.md)hakkında daha fazla bilgi edinin.

## <a name="sending-device-to-cloud-messages-to-iot-hub"></a>IoT Hub için cihazdan buluta iletiler gönderiliyor

IoT Hub, arka uç hizmetleri tarafından cihazlarınızdan telemetri iletilerini okumak için kullanılabilen yerleşik bir hizmet uç noktası vardır. Bu uç nokta [Event Hubs](../event-hubs/index.yml) uyumludur ve [Bu yerleşik uç noktadan okumak](iot-hub-devguide-messages-read-builtin.md)için standart IoT Hub SDK 'larını kullanabilirsiniz.

IoT Hub Ayrıca, kullanıcılar tarafından [ileti yönlendirme](iot-hub-devguide-messages-d2c.md)kullanarak cihaz telemetri verilerini ve olayları Azure hizmetlerine göndermek için tanımlanabilen [Özel uç noktaları](iot-hub-devguide-endpoints.md#custom-endpoints) da destekler.

## <a name="sending-cloud-to-device-messages-from-iot-hub"></a>IoT Hub 'ten cihazdan cihaza iletiler gönderiliyor

Çözüm arka ucundan cihazlarınıza [buluttan cihaza](iot-hub-devguide-messages-c2d.md) iletiler gönderebilirsiniz.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub mesajlaşma işlevinin temel özellikleri, iletilerin güvenilirliğini ve dayanıklılığını artırır. Bu özellikler, esnekliği cihaz tarafında aralıklı bağlantı kurmasını ve bulut tarafında olay İşlemede ani artışlar yüklemeyi sağlar. IoT Hub, cihazdan buluta ve buluttan cihaza mesajlaşma için *en az bir kez* teslim garantisi uygular.

## <a name="choosing-the-right-type-of-iot-hub-messaging"></a>IoT Hub mesajlaşma doğru türünü seçme

Cihaz uygulamanızdan zaman serisi telemetri ve uyarılar göndermek için cihazdan buluta iletileri ve cihaz uygulamanıza tek yönlü bildirimler için buluttan cihaza mesajlar kullanın.

* Cihazdan buluta iletiler, bildirilen özellikler veya karşıya dosya yükleme arasında seçim yapmak için [cihazdan buluta iletişim kılavuzuna](./iot-hub-devguide-d2c-guidance.md) bakın.

* Buluttan cihaza iletiler, istenen özellikler veya doğrudan yöntemler arasından seçim yapmak için [buluttan cihaza iletişim kılavuzuna](./iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub [ileti yönlendirme](iot-hub-devguide-messages-d2c.md)hakkında bilgi edinin.

* [Buluttan cihaza mesajlaşma](iot-hub-devguide-messages-c2d.md)IoT Hub hakkında bilgi edinin.