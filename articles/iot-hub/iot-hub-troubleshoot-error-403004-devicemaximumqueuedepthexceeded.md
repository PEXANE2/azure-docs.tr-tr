---
title: Sorun Giderme Azure IoT Hub hatası 403004 DeviceMaximumQueueDepthExceeded
description: Hata 403004 DeviceMaximumQueueDepthExceeded nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 1edf723aa885ff18d2ce2dda4d71b67700a98a5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497485"
---
# <a name="403004-devicemaximumqueuedepthexceeded"></a>403004 DeviceMaximumQueueDepthExceeded

Bu **makalede, 403004 DeviceMaximumQueueDepthExceeded** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Buluttan cihaza ileti göndermeye çalışırken, istek **403004** veya **DeviceMaximumQueueDepthExceeded**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

Bunun altında yatan neden, aygıt için sıraya giren ileti sayısının [sıra sınırını (50)](./iot-hub-devguide-quotas-throttling.md#other-limits)aşmasıdır.

Bu sınıra girmenizin en olası nedeni, iletiyi almak için HTTPS kullanıyor olmasıdır `ReceiveAsync`ve bu da ioT Hub'ın isteği azaltmasına neden olan sürekli yoklamalara yol açar.

## <a name="solution"></a>Çözüm

HTTPS ile buluttan aygıta iletiler için desteklenen desen, iletileri seyrek olarak kontrol eden aralıklı olarak bağlanan aygıtlardır (her 25 dakikadan az). Sıra sınırına girme olasılığını azaltmak için, buluttan cihaza iletiler için AMQP veya MQTT'ye geçin.

Alternatif olarak, sıraya alınan iletileri hızla tamamlamak, reddetmek veya terk etmek, yaşama süresini kısaltmak veya daha az ileti göndermeyi düşünmek için aygıt yan mantığını geliştirin. Bkz. [C2D iletisi yaşam süresi](./iot-hub-devguide-messages-c2d.md#message-expiration-time-to-live).

Son olarak, sınıra ulaşılmadan önce bekleyen iletileri düzenli aralıklarla temizlemek için [Temizleme Sırası API'sını](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/purgecommandqueue) kullanmayı düşünün.