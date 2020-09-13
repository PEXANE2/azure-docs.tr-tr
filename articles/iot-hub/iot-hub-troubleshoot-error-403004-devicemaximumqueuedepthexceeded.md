---
title: Azure IoT Hub sorun giderme hatası 403004 Devicemaximumqueuedepthaşıldı
description: 403004 Devicemaximumqueuedepthaşıldı hatasını nasıl düzelteceğinizi anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: fc5029f26e5d615502925c4def4e2973c118f38d
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90029996"
---
# <a name="403004-devicemaximumqueuedepthexceeded"></a>403004 DeviceMaximumQueueDepthExceeded

Bu makalede, **403004 Devicemaximumqueuedepthaşıldı** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Buluttan cihaza ileti gönderilmeye çalışırken, istek **403004** veya **Devicemaximumqueuedepthaşıldı**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

Temel neden, cihaz için sıraya alınan ileti sayısının [sıra sınırını (50)](./iot-hub-devguide-quotas-throttling.md#other-limits)aşmaktadır.

Bu sınırın üzerinde çalışmakta olduğunuz en olası nedeni, iletiyi almak için HTTPS 'yi kullanırken, sürekli yoklamaya yol açar ve bu da `ReceiveAsync` isteği azaltmayı IoT Hub.

## <a name="solution"></a>Çözüm

HTTPS ile buluttan cihaza iletiler için desteklenen model, iletileri seyrek olarak denetleyen (25 dakikada bir daha az) zaman zaman bağlı cihazlardır. Kuyruk sınırına çalışma olasılığını azaltmak için, buluttan cihaza iletiler için AMQP veya MQTT ' ye geçin.

Alternatif olarak, sıraya alınan iletileri hızla tamamlamayı, reddetmesini veya iptal etmeyi, canlı kalma süresini kısaltmaya veya daha az ileti göndermeyi deneyin. Bkz. [C2D iletisi yaşam süresi](./iot-hub-devguide-messages-c2d.md#message-expiration-time-to-live).

Son olarak, sınıra ulaşılmadan önce bekleyen iletileri düzenli aralıklarla temizlemek için [Temizleme kuyruğu API](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-registrymanager-h/iothubregistrymanager-deletedevice) 'sini kullanmayı düşünün.