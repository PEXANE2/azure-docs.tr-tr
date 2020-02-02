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
ms.openlocfilehash: d48fd9aa9ba52c850a514d392f25b980d0219470
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960912"
---
# <a name="403004-devicemaximumqueuedepthexceeded"></a>403004 Devicemaximumqueuedepthaşıldı

Bu makalede, **403004 Devicemaximumqueuedepthaşıldı** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Buluttan cihaza ileti gönderilmeye çalışırken, istek **403004** veya **Devicemaximumqueuedepthaşıldı**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

Temel neden, cihaz için sıraya alınan ileti sayısının [sıra sınırını (50)](./iot-hub-devguide-quotas-throttling.md#other-limits)aşmaktadır.

Bu limite çalıştırdığınız en olası neden, iletiyi almak için HTTPS 'yi kullanırken `ReceiveAsync`kullanarak sürekli yoklamaya yol açar ve isteği IoT Hub azaltmaya neden olur.

## <a name="solution"></a>Çözüm

HTTPS ile buluttan cihaza iletiler için desteklenen model, iletileri seyrek olarak denetleyen (25 dakikada bir daha az) zaman zaman bağlı cihazlardır. Kuyruk sınırına çalışma olasılığını azaltmak için, buluttan cihaza iletiler için AMQP veya MQTT ' ye geçin.

Alternatif olarak, sıraya alınan iletileri hızla tamamlamayı, reddetmesini veya iptal etmeyi, canlı kalma süresini kısaltmaya veya daha az ileti göndermeyi deneyin. Bkz. [C2D iletisi yaşam süresi](./iot-hub-devguide-messages-c2d.md#message-expiration-time-to-live).

Son olarak, sınıra ulaşılmadan önce bekleyen iletileri düzenli aralıklarla temizlemek için [Temizleme kuyruğu API](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) 'sini kullanmayı düşünün.