---
title: Sorun giderme Azure IoT Hub hatası 409002 LinkCreationConflict
description: Hata 409002 LinkCreationConflict nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: amqp
ms.openlocfilehash: 20d39b1f5a11f20eb5d12f34337787b382c820f6
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758747"
---
# <a name="409002-linkcreationconflict"></a>409002 LinkCreationConflict

Bu makalede, **409002 LinkCreationConflict** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

**409002 LinkCreationConflict** hatasını aygıt bağlantısının kesilmesi veya aygıttan aygıta ileti hatasıyla birlikte tanılama günlüklerinde günlüğe kaydedildiğini görürsünüz. 

<!-- When using AMQP? -->

## <a name="cause"></a>Nedeni

Genellikle, Bu hata IoT Hub bir istemci birden fazla bağlantı olduğunu algılar zaman olur. Aslında, varolan bağlantısı olan bir aygıt için yeni bir bağlantı isteği geldiğinde, IoT Hub varolan bağlantıyı bu hatayla kapatır.

### <a name="cause-1"></a>Neden 1

En yaygın durumda, ayrı bir sorun [(404104 DeviceConnectionConnectionRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)gibi) aygıtın bağlantısını kesmesine neden olur. Aygıt bağlantıyı hemen yeniden kurmaya çalışır, ancak IoT Hub aygıtın bağlı olduğunu düşünür. IoT Hub önceki bağlantıyı kapatır ve bu hatayı kaydeder.

### <a name="cause-2"></a>Neden 2

Hatalı aygıt tarafı mantığı, aygıtın zaten açıkken bağlantıyı kurmasını sağlar.

## <a name="solution"></a>Çözüm

Bu hata genellikle farklı, geçici bir sorunun yan etkisi olarak görünür, bu nedenle daha fazla sorun gidermek için günlükleri diğer hataları arayın. Aksi takdirde, yalnızca bağlantı düşerse yeni bir bağlantı isteği yayımlayımutlaka.
