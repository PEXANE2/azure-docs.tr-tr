---
title: Azure IoT Hub hatası 409002 LinkCreationConflict sorunlarını giderme
description: 409002 LinkCreationConflict hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: amqp
ms.openlocfilehash: 70b9be6fdb500d9f877659a12e6fdc0e206ea964
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92538230"
---
# <a name="409002-linkcreationconflict"></a>409002 LinkCreationConflict

Bu makalede, **409002 LinkCreationConflict** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Cihaz bağlantısının kesilmesi veya buluttan cihaza ileti hatası ile birlikte günlüklerde **409002 LinkCreationConflict** hatasını görürsünüz.

<!-- When using AMQP? -->

## <a name="cause"></a>Nedeni

Genellikle, IoT Hub istemcinin birden fazla bağlantısı olduğunu algıladığında bu hata oluşur. Aslında, var olan bağlantısı olan bir cihaza yeni bir bağlantı isteği ulaştığında, IoT Hub mevcut bağlantıyı bu hatayla kapatır.

### <a name="cause-1"></a>Neden 1

En yaygın durumda, ayrı bir sorun (örneğin, [404104 Deviceconnectioncloseduzaktan](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)) cihazın bağlantısının kesilip kesilmesine neden olur. Cihaz bağlantıyı hemen yeniden kurmaya çalışır, ancak IoT Hub cihazın bağlı olduğunu kabul eder. IoT Hub önceki bağlantıyı kapatır ve bu hatayı günlüğe kaydeder.

### <a name="cause-2"></a>Neden 2

Hatalı cihaz tarafı mantığı, cihazın zaten açık olduğu bir bağlantı kurmasını sağlar.

## <a name="solution"></a>Çözüm

Bu hata genellikle farklı, geçici bir sorunun yan etkisi olarak görünür, bu nedenle daha fazla sorun gidermek için günlüklerdeki diğer hatalara bakın. Aksi takdirde, yalnızca bağlantı düşerse yeni bir bağlantı isteği yayınlediğinizden emin olun.
