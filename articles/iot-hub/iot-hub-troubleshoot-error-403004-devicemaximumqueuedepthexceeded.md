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
ms.openlocfilehash: 5cc8bae0f0245f5c4b45ca0cd446582b04788c21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81758759"
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

Son olarak, sınıra ulaşılmadan önce bekleyen iletileri düzenli aralıklarla temizlemek için [Temizleme kuyruğu API](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/purgecommandqueue) 'sini kullanmayı düşünün.